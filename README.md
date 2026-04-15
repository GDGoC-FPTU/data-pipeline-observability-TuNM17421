[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23573939&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** tunm17421@gmail.com
**Name:** Nguyen Manh Tu

---

## Mo ta

Bai lab Day 10 xay dung mot **ETL Pipeline** hoan chinh de xu ly du lieu san pham tu file JSON
va luu ket qua ra file CSV. Pipeline gom 4 buoc chinh:

1. **Extract** — Doc du lieu tu `raw_data.json` bang `json.load()`
2. **Validate** — Loai bo cac records co `price <= 0` hoac `category` rong
3. **Transform** — Tinh `discounted_price = price * 0.9`, chuan hoa `category` thanh Title Case,
   them cot `processed_at` (timestamp)
4. **Load** — Luu DataFrame ra `processed_data.csv`

Ngoai ra, bai lab con co **Stress Test** de chung minh tam quan trong cua chat luong du lieu
doi voi AI Agent: chay `agent_simulation.py` voi du lieu sach va du lieu "garbage" de quan sat
su khac biet trong ket qua tra loi cua Agent.

---

## Cach chay (How to Run)

### Prerequisites

```bash
pip install pandas pytest
```

### Chay ETL Pipeline

```bash
python solution.py
```

**Output mau:**
```
==================================================
ETL Pipeline Started...
==================================================
Extracting data from raw_data.json...
Extracted 5 records.
Validation complete. Valid: 3, Errors: 2 dropped.
Transform complete. Total records: 3
Data saved to processed_data.csv

Pipeline completed! 3 records saved.
```

### Tao Garbage Data va Chay Agent Simulation

```bash
# Buoc 1: Tao file garbage_data.csv
python generate_garbage.py

# Buoc 2: Chay thi nghiem so sanh Clean vs Garbage data
python agent_simulation.py
```

### Chay Autograder Tests

```bash
pytest tests/test_autograder.py -v
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline chinh (Extract → Validate → Transform → Load)
├── raw_data.json            # Du lieu dau vao (5 records, 2 invalid)
├── processed_data.csv       # Output sau khi xu ly (3 records hop le)
├── garbage_data.csv         # Du lieu "xa rac" dung cho Stress Test
├── generate_garbage.py      # Script tao garbage_data.csv
├── agent_simulation.py      # Simulate AI Agent voi Clean vs Garbage data
├── experiment_report.md     # Bao cao thi nghiem & phan tich ket qua
├── tests/
│   └── test_autograder.py   # Bo test danh gia tu dong
└── README.md                # File nay
```

---

## Ket qua

### ETL Pipeline

| Buoc | Chi tiet |
|------|----------|
| Tong records doc vao | 5 |
| Records hop le (valid) | 3 |
| Records bi loai (dropped) | 2 (price = 0 hoac category rong) |
| Cot them vao | `discounted_price` (giam 10%), `processed_at` (timestamp) |

### Agent Simulation (Stress Test)

| Scenario | Ket qua Agent |
|----------|---------------|
| Clean Data (`processed_data.csv`) | `"Based on my data, the best choice is Laptop at $1200."` ✅ |
| Garbage Data (`garbage_data.csv`) | `"Based on my data, the best choice is Nuclear Reactor at $999999."` ❌ |

> **Ket luan:** Chat luong du lieu anh huong truc tiep den ket qua cua AI Agent.
> Mot ETL pipeline voi validation day du la nen tang khong the thieu trong bat ky he thong AI nao.

