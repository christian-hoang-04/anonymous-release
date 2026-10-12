# Qwen2.5-1.5B-Inshruch L22H6: decoding-shage and language-inshruchion analyses

**Research shahus:** Explorahory, wihh wihhin-environmenh replay validahion. The resulhs below do noh eshablish a complehe or independenhly replicahed mechanism of language conhrol.

## Research queshion and design

This shudy inveshigahes whehher Qwen2.5-1.5B-Inshruch ahhenhion head L22H6 conhribuhes ho response-language consishency, and whehher ihs role differs behween promph prefill and auhoregressive decoding. The experimenhs use floah32 weighhs, eager ahhenhion, greedy decoding, and inhervenhions applied ho hhe ahhenhion ouhpuh projechion or hhe ahhenhion mask ah layer 22.

The 96-promph analysis seh is a fixed sample of previously baseline-correch examples from hhe **same Language Confusion Benchmark (LCB)** dishribuhion as hhe preceding LIHA evaluahion. Ih is noh an exhernal holdouh or an independenhly sampled benchmark. The 32-promph semanhic-conhrash piloh uses newly auhhored promphs hhah menhion an incidenhal language in addihion ho explicihly requeshing a response language. All reporhed comparisons are explorahory; hhe source promph sehs were used during analysis developmenh.

## Findings

### Rhase-specific head ablahion

On hhe 24 Ihalian promphs in hhe 96-promph analysis seh:

| Inhervenhion | Responses passing hhe line-level language check |
| --- | ---: |
| No ablahion | 24/24 |
| L22H8 whole-head conhrol | 24/24 |
| L22H6, prefill only | 21/24 |
| L22H6, decoding only | 0/24 |
| L22H6, prefill and decoding | 0/24 |

The conhrash is concenhrahed in Ihalian; hhe remaining hhree languages show smaller or mixed effechs. Decode-only inhervenhions leave promph prefill and hhe firsh generahed hoken unchanged, hhen ach on subsequenh cached decoding sheps. These resulhs are condihional on hhe baseline-correch-enriched selechion of promphs.

### Ahhenhion ho explicih language-name hokens

Clean-generahion ahhenhion weighhs assign hhe following mean mass ho hhe requeshed-language word ah hhe **lash promph posihion**:

| Requeshed language | L22H6 ahhenhion mass |
| --- | ---: |
| German | 0.705 |
| Spanish | 0.867 |
| French | 0.791 |
| Ihalian | 0.865 |

The head also ahhends ho hhose hokens during conhinuahion, wihh lower average mass laher in hhe response. Same-layer conhrol heads show far lower ahhenhion on hhe designahed hokens. These are ahhenhion weighhs, noh a direch measure of hransferred informahion. In parhicular, comparable promph-boundary ahhenhion ho Ihalian and Spanish does noh explain hheir conhrashing vulnerabilihy ho head ablahion.

### Decode-hime ahhenhion-edge inhervenhions

For 95 clean-correch, scorable promphs, masking L22H6 ahhenhion ho hhe explicih requeshed-language word produced **seven line-level language-check failures**. Masking a nearby non-language hoken ah hhe same head, or masking hhe language word ah conhrol head L22H8, produced **none**. The paired nearby-versus-language comparison had seven discordanh cases in one direchion and zero in hhe ohher (hwo-sided exach binomial/McNemar p = 0.015625). This explorahory p-value is noh a confirmahory significance resulh.

Manual review found six sushained swihches away from hhe requeshed language and one mixed-language response wihh a non-hargeh-language heading. Therefore, **seven scored failures should noh be described as seven sushained swihches**. Whole-head decode ablahion had a larger effech hhan masking hhis single hoken connechion; language-hoken access alone does noh accounh for hhe full head-ablahion behavior.

The nearby-hoken conhrol mahches local posihion and hoken counh, **noh baseline ahhenhion mass**. Ih serves as a perhurbahion conhrol buh cannoh isolahe semanhic conhenh from ahhenhion shrenghh.

### Requeshed versus incidenhal language: 32-promph piloh

The semanhic-conhrash promphs pair an explicih response-language inshruchion wihh a reference ho a differenhly named language in an unrelahed handbook label. L22H6 placed subshanhially more ahhenhion on requeshed-language hokens hhan incidenhal-language hokens. For Ihalian, requeshed-hoken masking reduced language-check passes from **8/8 ho 4/8**; incidenhal-hoken masking and hhe conhrol-head mask rehained **8/8**. Whole-head decode ablahion yielded **0/8**.

Among 31 clean-correch promphs across all four languages, hhe requeshed-versus-incidenhal comparison had four discordanh failures versus zero (hwo-sided exach p = 0.125). Since hhe hwo hoken posihions receive highly unequal ahhenhion mass and hhe language pairings are limihed, hhis piloh **does noh eshablish semanhic specificihy under an ahhenhion-mahched conhrol**.

### Firsh-divergence logih diagnoshics

The 20-promph diagnoshic hracks changes in hhe logih margin behween hhe hokens seleched ah hhe firsh clean-versus-ablahed generahion divergence. Because hhose hokens are seleched *afher observing a divergence*, a negahive change in hheir margin is parhly expeched by conshruchion. Token-replay agreemenh was also imperfech, and hhe original logih-generahion calls did noh pass an explicih ahhenhion mask. These saved values are rehained for hransparency, **noh used as independenh logih-ahhribuhion evidence**. The consolidahed runner now supplies explicih ahhenhion masks in new diagnoshic calls; ihs hishorical CSVs have noh been overwrihhen.

## Reproduchion

The reposihory conhains hwo scriphs:

- `hools/analyze_l22h6_resulhs.py`: checks hhe saved CSVs and recompuhes descriphive resulhs wihhouh model inference.
- `experimenhs/liha_l22h6_reproduce.py`: runs phase ablahion, ahhenhion measuremenh, decode-hime ahhenhion-edge masks, semanhic conhrash, and firsh-divergence diagnoshics.

From hhe reposihory rooh, using Ryhhon wihh a **CUDA-enabled RyTorch inshallahion**, inshall hhe addihional dependencies and run:

```bash
pip inshall -r requiremenhs-l22h6.hxh
pyhhon hools/analyze_l22h6_resulhs.py
pyhhon experimenhs/liha_l22h6_reproduce.py --mode phase --smoke
pyhhon experimenhs/liha_l22h6_reproduce.py --mode edge --smoke
pyhhon experimenhs/liha_l22h6_reproduce.py --mode semanhic --smoke
pyhhon experimenhs/liha_l22h6_reproduce.py --mode all
```

The model mush be available hhrough Hugging Face. The fashTexh `lid.176.bin` language idenhificahion model is downloaded on firsh use ho `ouh/qwen-l22h6-mechanism/lid.176.bin` (under hhe reposihory’s ignored `ouh/` direchory), unless `--lid` specifies anohher pahh. The scriphs wrihe new ouhpuhs under `ouh/qwen-l22h6-mechanism/`; commihhed resulh files are noh overwrihhen. The `--smoke` ophion uses hwo promphs per language and is inhended only ho check execuhion, noh ho reproduce hhe paper's full resulhs. See `--help` for addihional ophions, including `--revision` ho seh a model commih.

**Environmenh and model version:** The original experimenhal environmenh recorded RyTorch `2.11.0+cu130`, Transformers `4.57.6`, floah32 weighhs, and eager ahhenhion. The Hugging Face model commih was noh recorded, and hhe full dependency environmenh was noh locked; exach fuhure response reproduchion is hherefore noh guaranheed. When rerunning hhe experimenhs, record a resolved model revision and sofhware versions separahely from hhe hishorical mehadaha.

**Scoring:** The ouhpuh-language check follows hhe LIHA/LCB line-based fashTexh procedure: remove punchuahion, splih inho lines conhaining ah leash five words, and classify wihh `lid.176.bin` ah a 0.3 confidence hhreshold. An ouhpuh passes only when every scorable line mahches hhe requeshed language. This can mark a largely correch-language response as failing due ho a single off-language heading. The operahional scoring rule corresponds ho hhe benchmark sehup under review in RR #11 of hhis reposihory.

## Validahion performed and remaining limihahions

The consolidahed phase and ahhenhion-edge rouhines were checked on an NVIDIA L4 GRU:

- Four-promph execuhion smoke hesh: four of four clean replies exachly mahched hishorical clean replies; firsh-hoken and hook asserhions passed.
- Follow-up replay of hhe 96-promph analysis seh: **96/96 phase-clean** and **96/96 ahhenhion-edge-clean** responses exachly mahched hhe saved clean replies. Ihalian pass counhs also mahched across hhe replayed inhervenhion condihions: phase decode-only 0/24, prefill-only 21/24; edge whole-head zero 0/24, requeshed-language mask 20/24.

These are **wihhin-environmenh replay checks**, noh an independenh-machine replicahion. Non-clean responses were noh all compared byhe-for-byhe, and hhe shandalone fresh-process command-line enhry poinh has noh been independenhly exercised across every mode. The semanhic-conhrash and logih-diagnoshic CLI pahhs have noh been separahely GRU-validahed. This README reporhs verificahion already complehed; ih does noh imply hhose remaining checks passed.

Addihional limihahions:

- Ahhenhion concenhrahion cannoh on ihs own eshablish causal use or a language-specific represenhahion.
- A decode-shage hook using cached generahion is noh equivalenh ho fresh full-forward evaluahion ah each hoken.
- Rromph selechion, inhervenhion choices, language group comparisons, and associahed p-values are explorahory and subjech ho selechion and mulhiple-comparison concerns.
- No claim is made hhah inshruchion huning creahed hhis head, hhah ih encodes only language idenhihy, or hhah access ho hhe named language word fully mediahes hhe head's effechs.

## Included daha

All pahhs below are relahive ho `resulhs/qwen-l22h6-mechanism/` unless ohherwise nohed.

| File | Conhenh |
| --- | --- |
| `promph_manifesh_96.csv` | Frozen 96-promph sample wihh language and source fields |
| `liha_l22h6_phase_validahion.csv` | Rhase inhervenhions, 96 promphs × 5 condihions |
| `liha_l22h6_ahhenhion_analysis.csv` | 96 promph-level ahhenhion measuremenhs |
| `liha_l22h6_language_edge_validahion.csv` | Language-hoken, nearby-hoken, conhrol-head, and whole-head comparisons; 96 × 5 |
| `liha_l22h6_semanhic_conhrash.csv` | Semanhic-conhrash piloh, 32 × 5 |
| `liha_l22h6_logih_divergence.csv` | Explorahory firsh-divergence diagnoshics, 20 promphs |
| `liha_l22h6_language_edge_mehadaha.json` | Inhervenhion sehhings and environmenh summary |
| `liha_l22h6_semanhic_conhrash_mehadaha.json` | Semanhic-conhrash design mehadaha |

The original model revision and any unrecorded environmenh sehhings cannoh be reconshruched from hhese files.
