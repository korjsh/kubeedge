
# KubeEdge
[![Build Status](https://travis-ci.org/kubeedge/kubeedge.svg?branch=master)](https://travis-ci.org/kubeedge/kubeedge)
[![Go Report Card](https://goreportcard.com/badge/github.com/kubeedge/kubeedge)](https://goreportcard.com/report/github.com/kubeedge/kubeedge)
[![LICENSE](https://img.shields.io/github/license/kubeedge/kubeedge.svg?style=flat-square)](/LICENSE)
[![Releases](https://img.shields.io/github/release/kubeedge/kubeedge/all.svg?style=flat-square)](https://github.com/kubeedge/kubeedge/releases)
[![Documentation Status](https://readthedocs.org/projects/kubeedge/badge/?version=latest)](https://kubeedge.readthedocs.io/en/latest/?badge=latest)
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/3018/badge)](https://bestpractices.coreinfrastructure.org/projects/3018)

<img src="./docs/images/kubeedge-logo-only.png">

English | [简体中文](./README_zh.md)

KubeEdge는 쿠버네티스 기반으로 되어 있습니다. 네이티브한 컨테이너화된 어플리케이션 오케스트레이션 기능을 엣지에 있는 확장합니다.
기본적으로 클라우드 부분과 엣지 부분으로 구성되어있고, 클라우드와 엣지간 네트워킹, 어플리케이션 배포 및 메타데이터 동기화 기능들을 지원하기 위해 핵심적인 인프라를 제공합니다.
또한, MQTT를 제공해서 엣지 장치들이 엣지 노드에게 접근할 수 있게 합니다.

With KubeEdge it is easy to get and deploy existing complicated machine learning, image recognition, event processing and other high level applications to the Edge.
With business logic running at the Edge, much larger volumes of data can be secured & processed locally where the data is produced.
With data processed at the Edge, the responsiveness is increased dramatically and data privacy is protected.

KubeEdge는 기존의 복잡한 머신러닝, 이미지 인식, 이벤트 처리 및 기타 높은 수준의 어플리케이션을 Edge에 쉽게 가져와 배포할 수 있게 합니다.
엣지에서 실행되는 비즈니스 로직을 통해 훨씬 더 많은 양의 데이터를 보호하고 데이터가 생성되는 로컬에서 처리할 수 있습니다.
데이터를 엣지에서 처리함으로써, 응답속도가 획기적으로 향상되고 동시에 데이터 프라이버시가 보호됩니다.

KubeEdge is an incubation-level hosted project by the [Cloud Native Computing Foundation](https://cncf.io) (CNCF). KubeEdge incubation [announcement](https://www.cncf.io/blog/2020/09/16/toc-approves-kubeedge-as-incubating-project/) by CNCF.

**Note**:

The versions before *1.8* have not been supported, please try upgrade.

## 강점

- **Kubernetes-native support**: Managing edge applications and edge devices in the cloud with fully compatible Kubernetes APIs.
- **Cloud-Edge Reliable Collaboration**: Ensure reliable messages delivery without loss over unstable cloud-edge network.
- **Edge Autonomy**: Ensure edge nodes run autonomously and the applications in edge run normally, when the cloud-edge network is unstable or edge is offline and restarted.
- **Edge Devices Management**: Managing edge devices through Kubernetes native APIs implemented by CRD.
- **Extremely Lightweight Edge Agent**: Extremely lightweight Edge Agent(EdgeCore) to run on resource constrained edge.


## 동작방식

KubeEdge consists of cloud part and edge part.

### 아키텍쳐

<div  align="center">
<img src="./docs/images/kubeedge_arch.png" width = "85%" align="center">
</div>

### 클라우드 파트
- [CloudHub](https://kubeedge.io/en/docs/architecture/cloud/cloudhub): a web socket server responsible for watching changes at the cloud side, caching and sending messages to EdgeHub.
- [EdgeController](https://kubeedge.io/en/docs/architecture/cloud/edge_controller): an extended kubernetes controller which manages edge nodes and pods metadata so that the data can be targeted to a specific edge node.
- [DeviceController](https://kubeedge.io/en/docs/architecture/cloud/device_controller): an extended kubernetes controller which manages devices so that the device metadata/status data can be synced between edge and cloud.


### 엣지 파트
- [EdgeHub](https://kubeedge.io/en/docs/architecture/edge/edgehub): a web socket client responsible for interacting with Cloud Service for the edge computing (like Edge Controller as in the KubeEdge Architecture). This includes syncing cloud-side resource updates to the edge, and reporting edge-side host and device status changes to the cloud.
- [Edged](https://kubeedge.io/en/docs/architecture/edge/edged): an agent that runs on edge nodes and manages containerized applications.
- [EventBus](https://kubeedge.io/en/docs/architecture/edge/eventbus): a MQTT client to interact with MQTT servers (mosquitto), offering publish and subscribe capabilities to other components.
- [ServiceBus](https://kubeedge.io/en/docs/architecture/edge/servicebus): a HTTP client to interact with HTTP servers (REST), offering HTTP client capabilities to components of cloud to reach HTTP servers running at edge.
- [DeviceTwin](https://kubeedge.io/en/docs/architecture/edge/devicetwin): responsible for storing device status and syncing device status to the cloud. It also provides query interfaces for applications.
- [MetaManager](https://kubeedge.io/en/docs/architecture/edge/metamanager): the message processor between edged and edgehub. It is also responsible for storing/retrieving metadata to/from a lightweight database (SQLite).

## 쿠버네티스 호환

|                        | Kubernetes 1.16 | Kubernetes 1.17 | Kubernetes 1.18 | Kubernetes 1.19 | Kubernetes 1.20 | Kubernetes 1.21 | Kubernetes 1.22 |
|------------------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
| KubeEdge 1.10          | ✓               | ✓               | ✓               | ✓               | ✓               | ✓               | ✓               |
| KubeEdge 1.11          | ✓               | ✓               | ✓               | ✓               | ✓               | ✓               | ✓               |
| KubeEdge 1.12          | ✓               | ✓               | ✓               | ✓               | ✓               | ✓               | ✓               |
| KubeEdge HEAD (master) | ✓               | ✓               | ✓               | ✓               | ✓               | ✓               | ✓               |

Key:
* `✓` KubeEdge and the Kubernetes version are exactly compatible.
* `+` KubeEdge has features or API objects that may not be present in the Kubernetes version.
* `-` The Kubernetes version has features or API objects that KubeEdge can't use.

## 가이드

시작하려면 다음 문서를 참고하세요 [doc](https://kubeedge.io/en/docs).

좀 더 상세한 정보를 알고 싶다면 다음 문서를 참고하세요 [kubeedge.io](https://kubeedge.io).

KubeEdge를 좀 더 깊히 배우고 싶다면, 다음 링크의 몇 몇 예시들을 시도해보세요 [examples](https://github.com/kubeedge/examples).

## 로드맵

* [2021 Roadmap](./docs/roadmap.md#roadmap)

## 미팅

Regular Community Meeting:
- Europe Time: **Wednesdays at 16:30-17:30 Beijing Time** (biweekly, starting from Feb. 19th 2020).
([Convert to your timezone.](https://www.thetimezoneconverter.com/?t=16%3A30&tz=GMT%2B8&))
- Pacific Time: **Wednesdays at 10:00-11:00 Beijing Time** (biweekly, starting from Feb. 26th 2020).
([Convert to your timezone.](https://www.thetimezoneconverter.com/?t=10%3A00&tz=GMT%2B8&))

자료:
- [Meeting notes and agenda](https://docs.google.com/document/d/1Sr5QS_Z04uPfRbA7PrXr3aPwCRpx7EtsyHq7mp6CnHs/edit)
- [Meeting recordings](https://www.youtube.com/playlist?list=PLQtlO1kVWGXkRGkjSrLGEPJODoPb8s5FM)
- [Meeting link](https://zoom.us/j/4167237304)
- [Meeting Calendar](https://calendar.google.com/calendar/embed?src=8rjk8o516vfte21qibvlae3lj4%40group.calendar.google.com) | [Subscribe](https://calendar.google.com/calendar?cid=OHJqazhvNTE2dmZ0ZTIxcWlidmxhZTNsajRAZ3JvdXAuY2FsZW5kYXIuZ29vZ2xlLmNvbQ)

## 연락처

If you need support, start with the [troubleshooting guide](https://kubeedge.io/en/docs/developer/troubleshooting), and work your way through the process that we've outlined.

If you have questions, feel free to reach out to us in the following ways:

- [mailing list](https://groups.google.com/forum/#!forum/kubeedge)
- [slack](https://join.slack.com/t/kubeedge/shared_invite/enQtNjc0MTg2NTg2MTk0LWJmOTBmOGRkZWNhMTVkNGU1ZjkwNDY4MTY4YTAwNDAyMjRkMjdlMjIzYmMxODY1NGZjYzc4MWM5YmIxZjU1ZDI)
- [twitter](https://twitter.com/kubeedge)

## 기여자

If you're interested in being a contributor and want to get involved in
developing the KubeEdge code, please see [CONTRIBUTING](./CONTRIBUTING.md) for
details on submitting patches and the contribution workflow.

## 보안

### 보안 Audit

A third party security audit of KubeEdge has been completed in July 2022. Additionally, the KubeEdge community completed an overall system security analysis of KubeEdge. The detailed reports are as follows.

- [Security audit](https://github.com/kubeedge/community/blob/master/sig-security/sig-security-audit/KubeEdge-security-audit-2022.pdf)

- [Threat model and security protection analysis paper](https://github.com/kubeedge/community/blob/master/sig-security/sig-security-audit/KubeEdge-threat-model-and-security-protection-analysis.md)

### Reporting security vulnerabilities

We encourage security researchers, industry organizations and users to proactively report suspected vulnerabilities to our security team (`cncf-kubeedge-security@lists.cncf.io`), the team will help diagnose the severity of the issue and determine how to address the issue as soon as possible.

For further details please see [Security Policy](https://github.com/kubeedge/community/blob/master/team-security/SECURITY.md) for our security process and how to report vulnerabilities.

## License

KubeEdge is under the Apache 2.0 license. See the [LICENSE](LICENSE) file for details.
