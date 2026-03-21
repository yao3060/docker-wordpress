# WordPress Benchmark Analysis (2026-03-22)

## Test Summary

- Tool: `ab (ApacheBench)`
- Total requests: `1000`
- Concurrency: `100`
- Entry points:
  - `wordpress_frankenphp`: `http://localhost/eu-en/`
  - `wordpress_apache`: `http://localhost/us-en/`
- Front proxy in both outputs: `nginx/1.27.5`

## Raw Result Comparison


| Metric                  | FrankenPHP   | Apache       | Diff (FrankenPHP vs Apache) |
| ----------------------- | ------------ | ------------ | --------------------------- |
| Time taken for tests    | 23.473 s     | 33.793 s     | **-30.54%**                 |
| Requests per second     | 42.60 req/s  | 29.59 req/s  | **+43.96%**                 |
| Time per request (mean) | 2347.340 ms  | 3379.342 ms  | **-30.54%**                 |
| Transfer rate           | 2858.04 KB/s | 1999.71 KB/s | **+42.92%**                 |
| Processing mean         | 2225 ms      | 3310 ms      | **-32.78%**                 |
| Waiting mean            | 2221 ms      | 3110 ms      | **-28.58%**                 |
| P95 latency             | 2414 ms      | 4997 ms      | **-51.69%**                 |
| P99 latency             | 2446 ms      | 5809 ms      | **-57.89%**                 |
| Max latency             | 2468 ms      | 6398 ms      | **-61.43%**                 |
| Failed requests         | 0            | 0            | Same                        |


## Key Findings

1. Under `100` concurrency, `wordpress_frankenphp` clearly outperforms `wordpress_apache` in both throughput and latency.
2. Average performance gains are significant:
  - Throughput (`RPS`) improves by about **44%**.
  - Mean request latency drops by about **31%**.
3. Tail latency gain is even larger:
  - `P95` improves by about **52%**.
  - `P99` improves by about **58%**.
  - This indicates better high-load stability and less long-tail delay.
4. Both tests have `0` failed requests, so the difference is not caused by error retries.

## Caveats (Fairness / Variables)

1. The compared URLs are different (`/eu-en/` vs `/us-en/`), and response sizes are not exactly the same:
  - 68454 bytes vs 68946 bytes (about `+0.72%`).
2. A/B tests should ideally use the same page content, same cache state, and same runtime warm-up condition.
3. Current results are single-run snapshots; repeating multiple rounds and taking median/percentile aggregates will be more reliable.

## Conclusion

Based on this test sample, `wordpress_frankenphp` shows a clear and meaningful performance advantage over `wordpress_apache`:

- Higher throughput
- Lower average latency
- Much better tail latency

If your production priority includes high-concurrency response time and latency stability, the current data supports preferring the FrankenPHP stack.

## Recommended Next Benchmark Steps

1. Use identical endpoint/content for both stacks (strict A/B).
2. Run at least `5-10` rounds per scenario and report median + `P95/P99`.
3. Add multi-level concurrency tests (for example: `c=20/50/100/200`).
4. Record resource metrics during runs (`CPU`, `memory`, `load average`, container stats).
5. Keep application/cache/database states aligned before each run.

