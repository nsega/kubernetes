# Active Issues in Kubernetes Codebase

## Summary
This document catalogs active issues found in the Kubernetes codebase that should be addressed.

## Issue Categories

### 1. klog.TODO() Usage (18 files)
These files use `klog.TODO()` as a placeholder for a proper logger. This indicates missing logger propagation.

**Priority: HIGH**

**Files affected:**
- `pkg/kubelet/stats/provider.go:127` - GetCgroupStats function
- `pkg/kubelet/kubelet_pods.go:2113` - convertToAPIContainerStatuses function
- `pkg/kubelet/prober/prober.go`
- `pkg/kubelet/kubelet.go`
- `pkg/kubelet/container/runtime.go`
- `pkg/kubelet/config/config.go`
- `pkg/kubelet/cm/memorymanager/memory_manager.go`
- `pkg/kubelet/cm/container_manager_linux.go`
- `pkg/kubelet/certificate/kubelet.go`
- `pkg/kubelet/cadvisor/cadvisor_windows.go`
- `pkg/controller/volume/persistentvolume/framework_test.go`
- `pkg/controller/volume/attachdetach/metrics/metrics.go`
- `staging/src/k8s.io/component-base/logs/example/example.go`
- `staging/src/k8s.io/client-go/transport/token_source.go`
- `staging/src/k8s.io/client-go/tools/cache/listers.go`
- `staging/src/k8s.io/apiextensions-apiserver/pkg/apiserver/apiserver.go`
- `pkg/scheduler/testing/framework/fake_extender.go`
- `pkg/scheduler/apis/config/v1/defaults.go`

**Recommended Fix:**
Add logger fields to structs and propagate loggers through function parameters properly.

### 2. context.TODO() Usage (591 files)
Widespread use of `context.TODO()` as a placeholder for proper context propagation.

**Priority: MEDIUM-HIGH**

**Impact:**
- Missing timeout/cancellation support
- Missing distributed tracing context
- Poor observability

**Recommended Fix:**
Systematically refactor functions to accept and propagate context.Context parameters.

### 3. Code TODO Comments (100+ instances)
Various TODO comments indicating incomplete work or needed improvements.

**Priority: VARIES**

**Notable TODOs:**

#### Build/Dependencies
- `build/dependencies.yaml:100` - Ensure newer versions get uploaded
- `build/dependencies.yaml:118` - Eliminate dependency controlled by .go-version

#### Kubelet
- `pkg/cluster/ports/ports.go:36` - Remove heapster workaround
- `pkg/kubelet/nodestatus/setters.go:217` - Post NotReady if cannot get MachineInfo from cAdvisor
- `pkg/kubelet/nodestatus/setters.go:221` - Required for test-cmd.sh to pass
- `pkg/kubelet/nodestatus/setters.go:245` - Needs transaction for node status update
- `pkg/kubelet/nodestatus/setters.go:256` - Use ContainerManager.GetCapacity for node resources
- `pkg/kubelet/kubelet_pods.go:932,951` - EnvFiles feature gate limits to relax
- `pkg/kubelet/kubelet_pods.go:981` - Remove line once all platforms use apiserver+Pods
- `pkg/kubelet/kubelet_pods.go:1109` - Extend PodDeletionSafetyProvider interface
- `pkg/kubelet/kubelet_pods.go:1219` - Logic doesn't handle two cases
- `pkg/kubelet/kubelet_pods.go:1296` - More aggressive cleanup of terminated pods
- `pkg/kubelet/kubelet_pods.go:1582` - Returning logs of random container attempts
- `pkg/kubelet/kubelet_pods.go:1809` - Assign terminal phase to static pods
- `pkg/kubelet/kubelet_pods.go:2433` - Handle in cleaner way
- `pkg/kubelet/kubelet_pods.go:2482` - Allowlist logs to serve
- `pkg/kubelet/kubelet_pods.go:2509` - Pass proper timeout value

#### Test Infrastructure
- `test/kubemark/start-kubemark.sh:90` - Migrate components to separate credentials
- `test/kubemark/start-kubemark.sh:223` - Replace with image preloading from ClusterLoader
- `cluster/log-dump/log-dump.sh:20` - Move script to test/e2e
- `cluster/log-dump/log-dump.sh:83` - Get rid of bash dependency sourcing
- `cluster/log-dump/log-dump.sh:322` - Handle rotated logs

#### Other Components
- `cmd/kubemark/app/hollow_node.go:85` - Refactor hollow-node into hollow-kubelet and hollow-proxy
- `pkg/kubelet/stats/cri_stats_provider_windows.go:200` - Add support for multiple network interfaces
- `build/lib/release.sh:148` - Docker images handling

## Recommended Action Plan

### Phase 1: High-Priority Quick Wins
1. Fix `klog.TODO()` usage in kubelet package (18 files)
   - Add logger fields to Provider struct
   - Update constructors to accept loggers
   - Propagate loggers through call chains

### Phase 2: Context Propagation
2. Systematically address `context.TODO()` usage
   - Start with critical paths (kubelet, API server)
   - Add context parameters to function signatures
   - Update all callers

### Phase 3: Code TODOs
3. Address high-priority TODO comments
   - kubelet pod lifecycle improvements
   - Test infrastructure cleanup
   - Build/dependency management

### Phase 4: Technical Debt
4. Refactor hollow-node architecture
5. Improve log handling and rotation
6. Clean up deprecated code paths

## Metrics
- **klog.TODO() instances**: 18 files
- **context.TODO() instances**: 591 files
- **TODO comments**: 100+ instances
- **Estimated effort**: Multiple sprints across multiple teams

## Next Steps
1. Prioritize issues based on impact and effort
2. Create tracking issues in GitHub for each major category
3. Assign ownership to appropriate teams
4. Schedule work across upcoming sprints
