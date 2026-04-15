# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600399
**Name:** Nguyen Manh Tu
**Date:** 2026-04-15

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Based on my data, the best choice is Laptop at $1200." | 10 | Ket qua chinh xac. Laptop la san pham electronics co gia cao nhat trong bo du lieu sach (da qua ETL pipeline). |
| Garbage Data (`garbage_data.csv`) | "Based on my data, the best choice is Nuclear Reactor at $999999." | 1 | Ket qua hoan toan sai. Agent chon san pham phi thuc te voi gia tri bat thuong do du lieu garbage chua outlier. |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Khi Agent su dung `garbage_data.csv`, no nhan duoc ket qua sai hoan toan vi bo du lieu nay
chua nhieu van de nghiem trong ve chat luong:

1. **Duplicate IDs (ID trung lap):** Record dau tien (id=1) bi trung lap — ca "Laptop" va
   "Banana" deu co id=1. Dieu nay khong duoc loc bo nen Agent thay nhieu san pham voi
   cung mot dinh danh, gay nhap nho trong logic lua chon.

2. **Wrong Data Types (Sai kieu du lieu):** Record "Broken Chair" co gia tri `price = "ten dollars"`
   la chuoi van ban thay vi so nguyen/thuc phan. Trong truong hop nay, pandas doc duoc gia tri
   nay nhung phep so sanh `idxmax()` co the bi loi hoac bo qua no.

3. **Outliers (Gia tri bat thuong):** "Nuclear Reactor" co gia $999,999 — day la outlier khong
   phu hop voi thuc te cua mot data store thuong mai. Vi Agent dung logic `idxmax()` (chon san
   pham co gia cao nhat), no se mac ket vao outlier nay va tra ve ket qua phi thuc te.

4. **Null / Empty Values (Gia tri trong):** Record "Ghost Item" co id trong, price = 0, va
   category trong. Du record nay co the bi loc boi ETL pipeline, garbage CSV khong duoc xu ly
   qua pipeline nen Agent doc thang file raw.

**Tom lai:** Agent chi "nghi" dua tren du lieu no doc vao. Neu du lieu xa rac, thieu validation,
thi du prompt co tot den dau, ket qua dau ra cung se sai. Day la bien chung ro rang nhat cho
van de "Garbage In, Garbage Out" trong cac he thong AI.

---

## 3. Ket luan

**Quality Data > Quality Prompt?** **Dong y.**

Du prompt duoc viet chinh xac va ro rang den dau, neu du lieu dau vao bi nhiem doc (garbage data),
Agent van se tra ve ket qua sai va co hai. Thi nghiem nay chung minh rang **chat luong du lieu
la nen tang quan trong nhat** trong bat ky he thong AI/ML nao. Mot ETL pipeline voi validation
day du — nhu `solution.py` da implement — dong vai tro "bo loc" bao ve Agent khoi cac du lieu
xau, tu do dam bao ket qua dau ra dang tin cay va co gia tri thuc te.
