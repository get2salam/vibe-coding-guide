# 📊 Example: React Dashboard

Building a complete analytics dashboard with vibe coding.

---

## The Goal

Build a modern analytics dashboard for a SaaS product.

---

## Session 1: Foundation

### User

```
Build an analytics dashboard with React + TypeScript.

Stack:
- Next.js 14 (App Router)
- Tailwind + shadcn/ui
- Recharts for graphs
- React Query for data fetching

Pages:
- Overview (key metrics + charts)
- Users (table with search/filter)
- Revenue (financial charts)

Start with the layout and overview page.
```

### Claude

I'll create a clean dashboard layout with the overview page.

```tsx
// app/layout.tsx
import { Inter } from 'next/font/google'
import './globals.css'
import { Sidebar } from '@/components/layout/Sidebar'
import { Header } from '@/components/layout/Header'
import { QueryProvider } from '@/providers/QueryProvider'

const inter = Inter({ subsets: ['latin'] })

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <QueryProvider>
          <div className="flex h-screen bg-gray-50">
            <Sidebar />
            <div className="flex-1 flex flex-col overflow-hidden">
              <Header />
              <main className="flex-1 overflow-y-auto p-6">
                {children}
              </main>
            </div>
          </div>
        </QueryProvider>
      </body>
    </html>
  )
}
```

```tsx
// components/layout/Sidebar.tsx
'use client'
import Link from 'next/link'
import { usePathname } from 'next/navigation'
import { cn } from '@/lib/utils'
import { 
  LayoutDashboard, 
  Users, 
  DollarSign, 
  Settings 
} from 'lucide-react'

const navItems = [
  { href: '/', label: 'Overview', icon: LayoutDashboard },
  { href: '/users', label: 'Users', icon: Users },
  { href: '/revenue', label: 'Revenue', icon: DollarSign },
  { href: '/settings', label: 'Settings', icon: Settings },
]

export function Sidebar() {
  const pathname = usePathname()
  
  return (
    <aside className="w-64 bg-white border-r">
      <div className="p-6">
        <h1 className="text-xl font-bold text-gray-800">Analytics</h1>
      </div>
      <nav className="px-3">
        {navItems.map(({ href, label, icon: Icon }) => (
          <Link
            key={href}
            href={href}
            className={cn(
              'flex items-center gap-3 px-3 py-2 rounded-lg mb-1 transition-colors',
              pathname === href 
                ? 'bg-blue-50 text-blue-600' 
                : 'text-gray-600 hover:bg-gray-100'
            )}
          >
            <Icon className="w-5 h-5" />
            {label}
          </Link>
        ))}
      </nav>
    </aside>
  )
}
```

```tsx
// app/page.tsx (Overview)
import { MetricCard } from '@/components/dashboard/MetricCard'
import { RevenueChart } from '@/components/dashboard/RevenueChart'
import { UserGrowthChart } from '@/components/dashboard/UserGrowthChart'
import { RecentActivity } from '@/components/dashboard/RecentActivity'

export default function OverviewPage() {
  return (
    <div className="space-y-6">
      <h2 className="text-2xl font-bold text-gray-800">Overview</h2>
      
      {/* Metric Cards */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        <MetricCard 
          title="Total Users" 
          value="12,459" 
          change="+12.5%" 
          trend="up" 
        />
        <MetricCard 
          title="Revenue" 
          value="$48,239" 
          change="+8.2%" 
          trend="up" 
        />
        <MetricCard 
          title="Active Now" 
          value="1,429" 
          change="-2.1%" 
          trend="down" 
        />
        <MetricCard 
          title="Conversion" 
          value="3.24%" 
          change="+0.5%" 
          trend="up" 
        />
      </div>
      
      {/* Charts Row */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <RevenueChart />
        <UserGrowthChart />
      </div>
      
      {/* Activity Feed */}
      <RecentActivity />
    </div>
  )
}
```

```tsx
// components/dashboard/MetricCard.tsx
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { TrendingUp, TrendingDown } from 'lucide-react'
import { cn } from '@/lib/utils'

interface MetricCardProps {
  title: string
  value: string
  change: string
  trend: 'up' | 'down'
}

export function MetricCard({ title, value, change, trend }: MetricCardProps) {
  return (
    <Card>
      <CardHeader className="pb-2">
        <CardTitle className="text-sm font-medium text-gray-500">
          {title}
        </CardTitle>
      </CardHeader>
      <CardContent>
        <div className="text-2xl font-bold">{value}</div>
        <div className={cn(
          'flex items-center gap-1 text-sm mt-1',
          trend === 'up' ? 'text-green-600' : 'text-red-600'
        )}>
          {trend === 'up' ? (
            <TrendingUp className="w-4 h-4" />
          ) : (
            <TrendingDown className="w-4 h-4" />
          )}
          {change}
        </div>
      </CardContent>
    </Card>
  )
}
```

```tsx
// components/dashboard/RevenueChart.tsx
'use client'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { 
  AreaChart, 
  Area, 
  XAxis, 
  YAxis, 
  CartesianGrid, 
  Tooltip, 
  ResponsiveContainer 
} from 'recharts'

const data = [
  { month: 'Jan', revenue: 4000 },
  { month: 'Feb', revenue: 3000 },
  { month: 'Mar', revenue: 5000 },
  { month: 'Apr', revenue: 4500 },
  { month: 'May', revenue: 6000 },
  { month: 'Jun', revenue: 5500 },
]

export function RevenueChart() {
  return (
    <Card>
      <CardHeader>
        <CardTitle>Revenue</CardTitle>
      </CardHeader>
      <CardContent>
        <ResponsiveContainer width="100%" height={300}>
          <AreaChart data={data}>
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis dataKey="month" />
            <YAxis />
            <Tooltip />
            <Area 
              type="monotone" 
              dataKey="revenue" 
              stroke="#3b82f6" 
              fill="#93c5fd" 
            />
          </AreaChart>
        </ResponsiveContainer>
      </CardContent>
    </Card>
  )
}
```

---

### User

```
Great foundation! Now make the metrics dynamic with React Query.
Add a hook that fetches from /api/metrics and show loading skeletons.
```

### Claude

```tsx
// hooks/useMetrics.ts
import { useQuery } from '@tanstack/react-query'

interface Metrics {
  totalUsers: { value: number; change: number }
  revenue: { value: number; change: number }
  activeNow: { value: number; change: number }
  conversion: { value: number; change: number }
}

async function fetchMetrics(): Promise<Metrics> {
  const res = await fetch('/api/metrics')
  if (!res.ok) throw new Error('Failed to fetch metrics')
  return res.json()
}

export function useMetrics() {
  return useQuery({
    queryKey: ['metrics'],
    queryFn: fetchMetrics,
    refetchInterval: 30000, // Refresh every 30s
  })
}
```

```tsx
// components/dashboard/MetricsGrid.tsx
'use client'
import { useMetrics } from '@/hooks/useMetrics'
import { MetricCard, MetricCardSkeleton } from './MetricCard'
import { formatCurrency, formatNumber, formatPercent } from '@/lib/format'

export function MetricsGrid() {
  const { data, isLoading, error } = useMetrics()
  
  if (isLoading) {
    return (
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        {[...Array(4)].map((_, i) => (
          <MetricCardSkeleton key={i} />
        ))}
      </div>
    )
  }
  
  if (error) {
    return (
      <div className="bg-red-50 text-red-600 p-4 rounded-lg">
        Failed to load metrics
      </div>
    )
  }
  
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
      <MetricCard 
        title="Total Users" 
        value={formatNumber(data!.totalUsers.value)}
        change={formatPercent(data!.totalUsers.change)}
        trend={data!.totalUsers.change >= 0 ? 'up' : 'down'}
      />
      <MetricCard 
        title="Revenue" 
        value={formatCurrency(data!.revenue.value)}
        change={formatPercent(data!.revenue.change)}
        trend={data!.revenue.change >= 0 ? 'up' : 'down'}
      />
      <MetricCard 
        title="Active Now" 
        value={formatNumber(data!.activeNow.value)}
        change={formatPercent(data!.activeNow.change)}
        trend={data!.activeNow.change >= 0 ? 'up' : 'down'}
      />
      <MetricCard 
        title="Conversion" 
        value={formatPercent(data!.conversion.value)}
        change={formatPercent(data!.conversion.change)}
        trend={data!.conversion.change >= 0 ? 'up' : 'down'}
      />
    </div>
  )
}
```

```tsx
// components/dashboard/MetricCard.tsx (updated with skeleton)
import { Skeleton } from '@/components/ui/skeleton'
// ... previous MetricCard code ...

export function MetricCardSkeleton() {
  return (
    <Card>
      <CardHeader className="pb-2">
        <Skeleton className="h-4 w-20" />
      </CardHeader>
      <CardContent>
        <Skeleton className="h-8 w-24 mb-2" />
        <Skeleton className="h-4 w-16" />
      </CardContent>
    </Card>
  )
}
```

```tsx
// app/api/metrics/route.ts
import { NextResponse } from 'next/server'

export async function GET() {
  // In real app, fetch from database
  const metrics = {
    totalUsers: { value: 12459, change: 12.5 },
    revenue: { value: 48239, change: 8.2 },
    activeNow: { value: 1429, change: -2.1 },
    conversion: { value: 3.24, change: 0.5 },
  }
  
  return NextResponse.json(metrics)
}
```

---

### User

```
Now build the Users page with:
- Data table using @tanstack/react-table
- Search input
- Status filter (active/inactive/all)
- Pagination
- Click row to see user detail modal
```

### Claude

```tsx
// app/users/page.tsx
'use client'
import { useState } from 'react'
import { useUsers } from '@/hooks/useUsers'
import { UsersTable } from '@/components/users/UsersTable'
import { UserSearch } from '@/components/users/UserSearch'
import { UserFilters } from '@/components/users/UserFilters'
import { UserModal } from '@/components/users/UserModal'
import { User } from '@/types'

export default function UsersPage() {
  const [search, setSearch] = useState('')
  const [status, setStatus] = useState<'all' | 'active' | 'inactive'>('all')
  const [page, setPage] = useState(1)
  const [selectedUser, setSelectedUser] = useState<User | null>(null)
  
  const { data, isLoading } = useUsers({ search, status, page })
  
  return (
    <div className="space-y-6">
      <h2 className="text-2xl font-bold text-gray-800">Users</h2>
      
      <div className="flex gap-4">
        <UserSearch value={search} onChange={setSearch} />
        <UserFilters status={status} onStatusChange={setStatus} />
      </div>
      
      <UsersTable 
        users={data?.users ?? []}
        isLoading={isLoading}
        onRowClick={setSelectedUser}
        pagination={{
          page,
          totalPages: data?.totalPages ?? 1,
          onPageChange: setPage,
        }}
      />
      
      <UserModal 
        user={selectedUser} 
        onClose={() => setSelectedUser(null)} 
      />
    </div>
  )
}
```

[... continues with full component implementations ...]

---

## Key Takeaways

1. **Built iteratively** - Layout → Static → Dynamic → Interactive
2. **Leveraged libraries** - shadcn/ui, React Query, TanStack Table
3. **Asked for specifics** - "with search/filter/pagination"
4. **Polished incrementally** - Added skeletons, error states, modals
