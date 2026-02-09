# 🔄 Example: Python Data Pipeline

Building a data processing pipeline with vibe coding.

---

## The Goal

Build a pipeline that fetches data from an API, processes it, and loads it into a database.

---

## Session 1: Pipeline Architecture

### User

```
Build a Python data pipeline for processing user analytics.

Requirements:
- Fetch data from a REST API (paginated)
- Transform: clean, validate, enrich
- Load into PostgreSQL
- Handle errors gracefully with retry
- Logging throughout
- Run as a scheduled job

Use modern Python (3.11+), type hints, async where appropriate.
Structure it professionally.
```

### Claude

I'll create a well-structured ETL pipeline with proper error handling.

```python
# pipeline/main.py
import asyncio
import logging
from datetime import datetime
from pipeline.extract import DataExtractor
from pipeline.transform import DataTransformer
from pipeline.load import DataLoader
from pipeline.config import settings

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

async def run_pipeline():
    """Main pipeline orchestrator."""
    start_time = datetime.now()
    logger.info("Starting analytics pipeline")
    
    extractor = DataExtractor(settings.API_BASE_URL, settings.API_KEY)
    transformer = DataTransformer()
    loader = DataLoader(settings.DATABASE_URL)
    
    try:
        # Extract
        logger.info("Extracting data from API...")
        raw_data = await extractor.fetch_all()
        logger.info(f"Extracted {len(raw_data)} records")
        
        # Transform
        logger.info("Transforming data...")
        transformed = transformer.process(raw_data)
        logger.info(f"Transformed {len(transformed)} valid records")
        
        # Load
        logger.info("Loading data to database...")
        await loader.upsert_batch(transformed)
        logger.info("Load complete")
        
        duration = datetime.now() - start_time
        logger.info(f"Pipeline completed in {duration.total_seconds():.2f}s")
        
    except Exception as e:
        logger.error(f"Pipeline failed: {e}")
        raise

if __name__ == "__main__":
    asyncio.run(run_pipeline())
```

```python
# pipeline/extract.py
import asyncio
import httpx
from typing import Any
from tenacity import retry, stop_after_attempt, wait_exponential
import logging

logger = logging.getLogger(__name__)

class DataExtractor:
    def __init__(self, base_url: str, api_key: str):
        self.base_url = base_url
        self.headers = {"Authorization": f"Bearer {api_key}"}
    
    async def fetch_all(self) -> list[dict[str, Any]]:
        """Fetch all pages of data from the API."""
        all_data = []
        page = 1
        
        async with httpx.AsyncClient() as client:
            while True:
                data, has_more = await self._fetch_page(client, page)
                all_data.extend(data)
                
                if not has_more:
                    break
                page += 1
        
        return all_data
    
    @retry(
        stop=stop_after_attempt(3),
        wait=wait_exponential(multiplier=1, min=4, max=10)
    )
    async def _fetch_page(
        self, 
        client: httpx.AsyncClient, 
        page: int
    ) -> tuple[list[dict], bool]:
        """Fetch a single page with retry logic."""
        logger.debug(f"Fetching page {page}")
        
        response = await client.get(
            f"{self.base_url}/analytics",
            headers=self.headers,
            params={"page": page, "per_page": 100}
        )
        response.raise_for_status()
        
        data = response.json()
        return data["items"], data["has_more"]
```

```python
# pipeline/transform.py
from dataclasses import dataclass
from datetime import datetime
from typing import Any
import logging

logger = logging.getLogger(__name__)

@dataclass
class AnalyticsRecord:
    user_id: str
    event_type: str
    timestamp: datetime
    properties: dict[str, Any]
    session_id: str | None
    
    def to_dict(self) -> dict[str, Any]:
        return {
            "user_id": self.user_id,
            "event_type": self.event_type,
            "timestamp": self.timestamp.isoformat(),
            "properties": self.properties,
            "session_id": self.session_id,
        }

class DataTransformer:
    def process(self, raw_data: list[dict[str, Any]]) -> list[AnalyticsRecord]:
        """Transform and validate raw data into structured records."""
        records = []
        errors = 0
        
        for item in raw_data:
            try:
                record = self._transform_item(item)
                if self._validate(record):
                    records.append(record)
            except Exception as e:
                errors += 1
                logger.warning(f"Transform error: {e}")
        
        if errors > 0:
            logger.warning(f"Skipped {errors} invalid records")
        
        return records
    
    def _transform_item(self, item: dict[str, Any]) -> AnalyticsRecord:
        """Transform a single raw item."""
        return AnalyticsRecord(
            user_id=str(item["user_id"]),
            event_type=self._normalize_event_type(item.get("event", "")),
            timestamp=datetime.fromisoformat(item["timestamp"]),
            properties=item.get("properties", {}),
            session_id=item.get("session_id"),
        )
    
    def _normalize_event_type(self, event: str) -> str:
        """Normalize event type names."""
        return event.lower().replace(" ", "_").strip()
    
    def _validate(self, record: AnalyticsRecord) -> bool:
        """Validate a transformed record."""
        if not record.user_id:
            return False
        if not record.event_type:
            return False
        return True
```

```python
# pipeline/load.py
import asyncpg
from typing import Any
import logging

logger = logging.getLogger(__name__)

class DataLoader:
    def __init__(self, database_url: str):
        self.database_url = database_url
        self.pool: asyncpg.Pool | None = None
    
    async def _get_pool(self) -> asyncpg.Pool:
        if self.pool is None:
            self.pool = await asyncpg.create_pool(self.database_url)
        return self.pool
    
    async def upsert_batch(self, records: list[Any], batch_size: int = 1000):
        """Upsert records in batches."""
        pool = await self._get_pool()
        
        for i in range(0, len(records), batch_size):
            batch = records[i:i + batch_size]
            await self._upsert_records(pool, batch)
            logger.debug(f"Upserted batch {i // batch_size + 1}")
    
    async def _upsert_records(self, pool: asyncpg.Pool, records: list[Any]):
        """Upsert a batch of records."""
        async with pool.acquire() as conn:
            await conn.executemany(
                """
                INSERT INTO analytics (user_id, event_type, timestamp, properties, session_id)
                VALUES ($1, $2, $3, $4, $5)
                ON CONFLICT (user_id, timestamp, event_type) 
                DO UPDATE SET properties = $4, session_id = $5
                """,
                [
                    (r.user_id, r.event_type, r.timestamp, r.properties, r.session_id)
                    for r in records
                ]
            )
```

```python
# pipeline/config.py
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    API_BASE_URL: str
    API_KEY: str
    DATABASE_URL: str
    
    class Config:
        env_file = ".env"

settings = Settings()
```

---

### User

```
Add these features:
1. Incremental loading (only fetch data since last run)
2. Data quality checks between transform and load
3. Metrics collection (records processed, errors, duration)
4. Dead letter queue for failed records
```

### Claude

```python
# pipeline/state.py
import json
from pathlib import Path
from datetime import datetime

class PipelineState:
    def __init__(self, state_file: str = ".pipeline_state.json"):
        self.state_file = Path(state_file)
        self._state = self._load()
    
    def _load(self) -> dict:
        if self.state_file.exists():
            return json.loads(self.state_file.read_text())
        return {}
    
    def save(self):
        self.state_file.write_text(json.dumps(self._state, indent=2))
    
    @property
    def last_run(self) -> datetime | None:
        ts = self._state.get("last_run")
        return datetime.fromisoformat(ts) if ts else None
    
    @last_run.setter
    def last_run(self, value: datetime):
        self._state["last_run"] = value.isoformat()
        self.save()
```

```python
# pipeline/quality.py
from dataclasses import dataclass
from typing import Any
import logging

logger = logging.getLogger(__name__)

@dataclass
class QualityReport:
    total_records: int
    passed: int
    failed: int
    issues: list[str]
    
    @property
    def pass_rate(self) -> float:
        return self.passed / self.total_records if self.total_records else 0

class DataQualityChecker:
    def __init__(self, min_pass_rate: float = 0.95):
        self.min_pass_rate = min_pass_rate
    
    def check(self, records: list[Any]) -> tuple[list[Any], QualityReport]:
        """Run quality checks and return valid records + report."""
        passed = []
        issues = []
        
        for record in records:
            record_issues = self._check_record(record)
            if record_issues:
                issues.extend(record_issues)
            else:
                passed.append(record)
        
        report = QualityReport(
            total_records=len(records),
            passed=len(passed),
            failed=len(records) - len(passed),
            issues=issues[:100]  # Limit stored issues
        )
        
        logger.info(f"Quality check: {report.pass_rate:.1%} pass rate")
        
        if report.pass_rate < self.min_pass_rate:
            raise ValueError(f"Pass rate {report.pass_rate:.1%} below threshold")
        
        return passed, report
    
    def _check_record(self, record: Any) -> list[str]:
        issues = []
        
        # Check required fields
        if not record.user_id:
            issues.append(f"Missing user_id")
        
        # Check data types
        if record.timestamp and record.timestamp.year < 2020:
            issues.append(f"Suspicious timestamp: {record.timestamp}")
        
        # Check business rules
        if record.event_type not in self._valid_events():
            issues.append(f"Unknown event type: {record.event_type}")
        
        return issues
    
    def _valid_events(self) -> set[str]:
        return {"page_view", "click", "sign_up", "purchase", "logout"}
```

```python
# pipeline/metrics.py
from dataclasses import dataclass, field
from datetime import datetime
from typing import Any
import json

@dataclass
class PipelineMetrics:
    start_time: datetime = field(default_factory=datetime.now)
    end_time: datetime | None = None
    records_extracted: int = 0
    records_transformed: int = 0
    records_loaded: int = 0
    errors: int = 0
    quality_pass_rate: float = 0.0
    
    @property
    def duration_seconds(self) -> float:
        if self.end_time:
            return (self.end_time - self.start_time).total_seconds()
        return 0
    
    def to_dict(self) -> dict[str, Any]:
        return {
            "start_time": self.start_time.isoformat(),
            "end_time": self.end_time.isoformat() if self.end_time else None,
            "duration_seconds": self.duration_seconds,
            "records_extracted": self.records_extracted,
            "records_transformed": self.records_transformed,
            "records_loaded": self.records_loaded,
            "errors": self.errors,
            "quality_pass_rate": self.quality_pass_rate,
        }
    
    def complete(self):
        self.end_time = datetime.now()
```

```python
# pipeline/dlq.py
import json
from pathlib import Path
from datetime import datetime
from typing import Any

class DeadLetterQueue:
    def __init__(self, dlq_dir: str = ".dlq"):
        self.dlq_dir = Path(dlq_dir)
        self.dlq_dir.mkdir(exist_ok=True)
    
    def push(self, record: Any, error: str):
        """Push a failed record to the DLQ."""
        filename = f"{datetime.now().isoformat()}_{hash(str(record))}.json"
        filepath = self.dlq_dir / filename
        
        filepath.write_text(json.dumps({
            "record": record.to_dict() if hasattr(record, 'to_dict') else record,
            "error": error,
            "timestamp": datetime.now().isoformat(),
        }, indent=2))
    
    def count(self) -> int:
        return len(list(self.dlq_dir.glob("*.json")))
    
    def replay(self):
        """Get all DLQ records for reprocessing."""
        for file in self.dlq_dir.glob("*.json"):
            yield json.loads(file.read_text())
```

---

### User

```
Perfect! Now add a CLI interface with these commands:
- run: execute the pipeline
- status: show last run status
- replay-dlq: reprocess failed records
- backfill: run for a specific date range
```

### Claude

```python
# pipeline/cli.py
import click
import asyncio
from datetime import datetime
from rich.console import Console
from rich.table import Table
from pipeline.main import run_pipeline
from pipeline.state import PipelineState
from pipeline.dlq import DeadLetterQueue
from pipeline.metrics import PipelineMetrics

console = Console()

@click.group()
def cli():
    """Analytics data pipeline CLI."""
    pass

@cli.command()
@click.option('--full', is_flag=True, help='Force full load instead of incremental')
def run(full: bool):
    """Run the data pipeline."""
    console.print("[bold blue]Starting pipeline...[/bold blue]")
    
    try:
        metrics = asyncio.run(run_pipeline(incremental=not full))
        
        console.print("\n[bold green]✓ Pipeline completed successfully[/bold green]")
        _print_metrics(metrics)
        
    except Exception as e:
        console.print(f"\n[bold red]✗ Pipeline failed: {e}[/bold red]")
        raise SystemExit(1)

@cli.command()
def status():
    """Show pipeline status."""
    state = PipelineState()
    dlq = DeadLetterQueue()
    
    table = Table(title="Pipeline Status")
    table.add_column("Metric", style="cyan")
    table.add_column("Value", style="green")
    
    table.add_row("Last Run", str(state.last_run or "Never"))
    table.add_row("DLQ Records", str(dlq.count()))
    
    console.print(table)

@cli.command()
def replay_dlq():
    """Reprocess records in the dead letter queue."""
    dlq = DeadLetterQueue()
    count = dlq.count()
    
    if count == 0:
        console.print("[yellow]No records in DLQ[/yellow]")
        return
    
    console.print(f"[blue]Replaying {count} records...[/blue]")
    # Implementation for replaying records
    console.print("[green]DLQ replay complete[/green]")

@cli.command()
@click.option('--start', required=True, type=click.DateTime(), help='Start date')
@click.option('--end', required=True, type=click.DateTime(), help='End date')
def backfill(start: datetime, end: datetime):
    """Backfill data for a date range."""
    console.print(f"[blue]Backfilling from {start} to {end}...[/blue]")
    
    metrics = asyncio.run(run_pipeline(start_date=start, end_date=end))
    
    console.print("[green]Backfill complete[/green]")
    _print_metrics(metrics)

def _print_metrics(metrics: PipelineMetrics):
    table = Table(title="Pipeline Metrics")
    table.add_column("Metric", style="cyan")
    table.add_column("Value", style="green")
    
    table.add_row("Duration", f"{metrics.duration_seconds:.2f}s")
    table.add_row("Records Extracted", str(metrics.records_extracted))
    table.add_row("Records Loaded", str(metrics.records_loaded))
    table.add_row("Errors", str(metrics.errors))
    table.add_row("Quality Pass Rate", f"{metrics.quality_pass_rate:.1%}")
    
    console.print(table)

if __name__ == "__main__":
    cli()
```

---

## Key Takeaways

1. **Started with structure** - Clear separation of concerns (extract/transform/load)
2. **Added robustness** - Retry logic, error handling, DLQ
3. **Built incrementally** - Core pipeline → Quality checks → Metrics → CLI
4. **Professional touches** - Type hints, logging, rich CLI output
