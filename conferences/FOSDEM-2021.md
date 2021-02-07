# FOSDEM 2021

I "attended" (virtually, thanks COVID-19) [FOSDEM 2021](https://fosdem.org/2021/) on Sat/Sun Feb 6/7, 2021.

https://fosdem.org/2021/schedule/ has the full schedule - and there's a lot, as with any big tech conference.

https://www.youtube.com/channel/UC9NuJImUbaSNKiwF2bdSfAw has (will have) recordings. Here are talk I listened in to:

## Cloud Infra

* https://fosdem.org/2021/schedule/event/containers_datacenter_class/ about how https://www.opencompute.org open design HW "2nd life" that can be bought (again; "circular IT HW") on https://www.itrenew.com/explore-solutions/ (pricing on http://bit.ly/sesame-specials; more on https://stands.fosdem.org/stands/sesame_discovery/) and managed with https://github.com/opencomputeproject and https://rackn.com/rebar/ (like https://metal3.io?) - at home? ;) I know what I want for christmas, LOL.

* https://fosdem.org/2021/schedule/event/monarch_open_source_reimplementation/ presents https://thanos.io for (on top or instead of?) Prometheus is https://www.vldb.org/pvldb/vol13/p3181-adams.pdf Google's Monarch.

* https://fosdem.org/2021/schedule/event/getting_started_tempo/ presents https://grafana.com/oss/tempo/ and shows this https://github.com/joe-elliott/tracing-example demo of about to be merged support for _Exemplars_ letting you drill down from metrics graphs into related logs.

* https://fosdem.org/2021/schedule/event/k8sconfigmgmtlandscape/
  * https://fosdem.org/2021/schedule/event/k8sconfigmgmtlandscape/attachments/slides/4594/export/events/attachments/k8sconfigmgmtlandscape/slides/4594/2021_02_Kubernetes_Config_Management_Landscape.pdf
  * https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/declarative-application-management.md
  * https://storage.googleapis.com/pub-tools-public-publication-data/pdf/44843.pdf

* https://fosdem.org/2021/schedule/event/gitops_working_group/

* https://fosdem.org/2021/schedule/event/containers_toolbox/ slides https://fosdem.org/2021/schedule/event/containers_toolbox/attachments/slides/4564/export/events/attachments/containers_toolbox/slides/4564/By_the_Power_of_Toolbox_Faggioli_FOSDEM21.pdf

* https://fosdem.org/2021/schedule/event/developer_perspective_on_immutable_os/
  * https://fosdem.org/2021/schedule/event/developer_perspective_on_immutable_os/attachments/slides/4566/export/events/attachments/developer_perspective_on_immutable_os/slides/4566/User_Dev_Perspective_on_Immutable_OSes_Faggioli_FOSDEM21.pdf
  * https://github.com/dfaggioli/gdm-container

* https://fosdem.org/2021/schedule/event/cloud_kube_scheduler/

* https://fosdem.org/2021/schedule/event/clusterapiascode/

* https://fosdem.org/2021/schedule/event/alternativeherokuendtoendopensourceinfraautotoolchain/
  * https://docs.google.com/presentation/d/1olMXBvMIM5iFqSapA3HXGTp7qAHqG6CwmWoOFcuMX0E/edit?usp=sharing


## dWeb/web3 - The future distributed Web on the decentralized Internet?

https://fosdem.org/2021/schedule/track/beyond_blockchain_distributed_web:

* https://fosdem.org/2021/schedule/event/open_research_filecoin_ipfs/
  * https://fosdem.org/2021/schedule/event/open_research_filecoin_ipfs/attachments/slides/4457/export/events/attachments/open_research_filecoin_ipfs/slides/4457/FOSDEM_Filecoin_IPFS_A_new_Home_for_Research_Data.pdf
  * https://libp2p.io
  * https://filecoin.io
  * https://docs.textile.io/powergate/
  * https://qri.io

* https://fosdem.org/2021/schedule/event/ipfs/
  * https://fosdem.org/2021/schedule/event/ipfs/attachments/slides/4590/export/events/attachments/ipfs/slides/4590/FOSDEM21_Beyond_Swapping_Bits.pdf
  * https://github.com/protocol/beyond-bitswap

* https://fosdem.org/2021/schedule/event/holochain_what_is_it/
  * https://holochain.org
  * https://developer.holochain.org
  * https://github.com/holochain
  * https://holo.host
  * https://github.com/holochain-open-dev/file-storage-module
  * https://store.holo.host/

* https://fosdem.org/2021/schedule/event/scuttlebutt_ecosystem_introduction/ stores messages locally, no IPFS like distribution, but if you keep your prviate key, you can get lost data back from friends.  only 1 identity per device, so phone and computer separate (like Threma), but that's being worked on.
  * https://scuttlebutt.nz
  * https://scuttlebot.io
  * https://github.com/ssbc/patchwork
  * https://github.com/ssbc/patchbay
  * https://www.manyver.se Android
  * Patchbox is another client
  * https://www.ahau.io

* https://fosdem.org/2021/schedule/event/fluence_aquamarine101/
  * https://fluence.network
  * https://github.com/fluencelabs/aqua-demo#fluence-stack
  * https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type
  * https://en.wikipedia.org/wiki/%CE%A0-calculus
  * https://github.com/topics/decentralization
  * WASM https://github.com/fluencelabs/wasmer
  * https://libp2p.io (IPFS)

* https://fosdem.org/2021/schedule/event/neuropil/ see https://neuropil.org (AKA https://twitter.com/neuropil1)


## alt.misc

* https://fosdem.org/2021/schedule/event/webassembly/ glimpse on WASM IR https://fosdem.org/2021/schedule/event/webassembly/attachments/slides/4287/export/events/attachments/webassembly/slides/4287/fosdem_2021_compiling_to_webassembly_slides.pdf and https://github.com/wingo/compiling-to-webassembly

* https://fosdem.org/2021/schedule/event/asciinema_honeypot/ slides https://fosdem.org/2021/schedule/event/asciinema_honeypot/attachments/slides/4666/export/events/attachments/asciinema_honeypot/slides/4666/Watch_the_Asciinema_Replay_of_Your_Home_Made_Honeypot.pdf; I like https://containerssh.io

* https://fosdem.org/2021/schedule/event/dep_early_warning_signs_for_open_source_breakages/ Rhys Arkins and I "met" virtually in the past, and we appreciate his https://www.whitesourcesoftware.com/free-developer-tools/renovate on https://github.com/apache/fineract/pulls?q=author%3Arenovate-bot+

* https://fosdem.org/2021/schedule/event/stateofgo/ Go 1.16 with spectre, io/fs, https://golangtutorial.dev/tips/embed-files-in-go/ et al


### Java

https://fosdem.org/2021/schedule/track/friends_of_openjdk:

* https://fosdem.org/2021/schedule/event/jfr/ I've used this extensively in a past life, and it has clearly evolved further since.

## Moar

* https://fosdem.org/2021/schedule/track/collaborative_information_and_content_management_applications/
* https://fosdem.org/2021/schedule/track/infra_management/
