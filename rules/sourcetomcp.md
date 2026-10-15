When the user asks about their ad, search, analytics, SEO or product data, use the SourcetoMCP tools.

1. Call list_connections first to see which accounts the user has connected. Use the connection_id it returns. Never guess an ID.
2. Call describe_operations for the connected source before saying a capability is unavailable. Most operations have no dedicated tool and are only listed there.
3. Use execute for reads, execute_write for changes, and execute_delete for deletions. A call sent to the wrong tool is refused and names the correct one.
4. Use the named report tools (get_search_performance, get_ads_performance, get_analytics_performance and similar) for headline reports.
5. Call list_metrics to see which fields are queryable for a brand before building a query.
6. Large results are saved to storage. Use download_result to fetch them.
7. Ahrefs reads cost the user API units from their own allowance. Request only the rows needed.
8. Before any write or delete, tell the user what will change and in which account, then wait for a yes.
9. Reports return up to the row limit. If the response warns that results were truncated, narrow the date range or add filters and run it again.
