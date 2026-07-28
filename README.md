# Drug Discovery Camp

2일 동안 진행한 구조 기반 신약 개발(Structure-Based Drug Discovery, SBDD) 실습 자료입니다. 단백질과 리간드의 3차원 구조를 준비하고, 결합 부위를 예측한 뒤 AutoDock Vina로 도킹 및 교차 도킹을 수행합니다.

## 실습 목표

- SMILES로부터 도킹 가능한 3D 리간드 구조 만들기
- PDB 구조를 내려받아 수용체를 정리하고 PDBQT로 변환하기
- 단백질의 형태만 사용해 결합 부위(binding site) 예측하기
- 공결정 구조를 템플릿으로 선택하고 도킹 결과 비교하기
- 교차 도킹으로 결합 점수, 포켓 중심 거리, self-docking RMSD 해석하기

## 구성

| 일차 | 노트북 | 내용 |
| --- | --- | --- |
| Day 1 | `Day_1_Session_2.ipynb` | 리간드 준비, 단백질 준비, 기본 분자 도킹 |
| Day 2-1 | `Day_2_Session_1_1_Binding_Site_Prediction.ipynb` | LIGSITE 방식의 결합 부위 예측 및 공결정 리간드와 비교 |
| Day 2-2 | `Day_2_Session_1_2_Template_Selection.ipynb` | Abl kinase 템플릿(1IEP/2GQG) 선택, Imatinib·Dasatinib 도킹, decoy 평가, AlphaFold 비교 |
| Day 2-3 | `Day_2_Session_2_2_Corss_Dock.ipynb` | 팀 템플릿을 이용한 교차 도킹 및 결과 지표 계산 |

강의 자료 PDF도 함께 포함되어 있습니다.

- `day1-session1.pdf`
- `day1-session2.pdf`
- `day2-session1.pdf`

## 빠른 시작

Python 3.10 이상과 Jupyter Notebook 또는 Google Colab 환경을 권장합니다. 각 노트북의 첫 번째 설치 셀을 실행한 후 위에서 아래 순서로 실행하세요. PDB 구조와 AlphaFold 모델을 내려받으므로 인터넷 연결이 필요합니다.

```bash
git clone <repository-url>
cd Drug-Discovery
python -m pip install jupyter
jupyter notebook
```

로컬 환경에서 주요 라이브러리를 미리 설치하려면 다음을 사용할 수 있습니다.

```bash
python -m pip install rdkit "meeko==0.7.1" vina gemmi py3Dmol numpy scipy
```

`Day_2_Session_2_2_Corss_Dock.ipynb`는 Linux용 AutoDock Vina 1.2.5 실행 파일을 자동으로 내려받습니다. macOS 또는 Windows에서 로컬 실행 시에는 해당 셀을 운영체제에 맞는 Vina 설치 경로로 바꾸거나 Google Colab에서 실행하세요.

## 권장 실행 순서

1. `Day_1_Session_2.ipynb`에서 SMILES → 3D conformer → PDBQT 변환 과정을 익힙니다.
2. `Day_2_Session_1_1_Binding_Site_Prediction.ipynb`에서 1IEP와 1ERE의 결합 포켓을 예측하고 실제 공결정 리간드 중심과 비교합니다.
3. `Day_2_Session_1_2_Template_Selection.ipynb`에서 1IEP(DFG-out)와 2GQG(DFG-in)를 비교해 목적 리간드에 적합한 템플릿을 선택합니다.
4. `Day_2_Session_2_2_Corss_Dock.ipynb`의 설정 셀에서 `TEAM_TEMPLATE`, `LIGAND_NAME`, `LIGAND_SMILES`를 팀 과제 값으로 바꾼 뒤 실행합니다.

## 교차 도킹 결과 읽기

| 지표 | 의미 | 해석 |
| --- | --- | --- |
| Vina score (kcal/mol) | 예측 결합 친화도 | 더 낮은(더 음수) 값일수록 유리한 예측 |
| Pocket distance (Å) | 도킹 pose와 공결정 리간드 중심의 거리 | 작을수록 동일 포켓 중심에 가까움 |
| RMSD (Å) | self-docking일 때 공결정 pose와의 차이 | 작을수록 공결정 pose 재현성이 높음 |
| ROC-AUC / Top-3 | active와 decoy를 구분하는 템플릿 성능 | 높을수록 virtual screening 템플릿으로 유리 |

도킹 점수는 실험적 결합 친화도 그 자체가 아니라 모델 기반 예측값입니다. 단백질 구조 상태, protonation, 탐색 박스, 리간드 conformer 등의 조건과 함께 해석해야 합니다.

## 주요 도구

- [RDKit](https://www.rdkit.org/): 분자 파싱, 3D conformer 생성, 구조 처리
- [Meeko](https://forli-lab.github.io/Meeko/): 리간드/수용체 PDBQT 준비
- [AutoDock Vina](https://autodock-vina.readthedocs.io/): 분자 도킹 및 점수 계산
- [Gemmi](https://gemmi.readthedocs.io/): PDB 구조 정리
- [py3Dmol](https://3dmol.csb.pitt.edu/): 노트북 내 3D 구조 시각화
- [RCSB PDB](https://www.rcsb.org/), [AlphaFold DB](https://alphafold.ebi.ac.uk/): 구조 데이터 소스

## 생성 파일 및 주의사항

실행 중 `.pdb`, `.pdbqt`, `.sdf`, `docked.*`, `*_clean.pdb`, `*_receptor.pdbqt` 등 중간 산출물이 현재 폴더에 생성됩니다. 재현 가능한 비교를 위해 노트북에서 사용한 random seed와 탐색 박스 설정을 유지하세요.

이 자료는 교육용 실습입니다. 후보물질의 실제 활성·안전성·개발 가능성을 판단하려면 실험 검증과 추가 계산이 필요합니다.
