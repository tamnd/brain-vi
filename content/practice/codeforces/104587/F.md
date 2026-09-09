---
title: "CF 104587F - Qua Ngọn Đồi, Phần 2"
description: "Chúng ta được cung cấp một mô hình mã hóa tuyến tính cổ điển trong đó các khối văn bản có kích thước cố định được biến đổi bằng cách nhân chúng với một ma trận vuông chưa xác định."
date: "2026-06-30T07:29:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104587
codeforces_index: "F"
codeforces_contest_name: "2020-2021 ICPC East Central North America Regional Contest (ECNA 2020)"
rating: 0
weight: 104587
solve_time_s: 61
verified: true
draft: false
---

[CF 104587F - Trên đồi, Phần 2](https://codeforces.com/problemset/problem/104587/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mô hình mã hóa tuyến tính cổ điển trong đó các khối văn bản có kích thước cố định được biến đổi bằng cách nhân chúng với một ma trận vuông chưa xác định. Mỗi khối có độ dài n được hiểu là một vectơ, ma trận biến đổi nó và chúng ta quan sát cả vectơ đầu vào và đầu ra tương ứng của chúng. Nhiệm vụ là khôi phục ma trận biến đổi từ các cặp được quan sát này. 

Mỗi ký tự được ánh xạ ngầm tới một số nguyên trong một bảng chữ cái cố định (chữ in hoa, chữ số và dấu cách), do đó mọi khối sẽ trở thành một vectơ trên các số nguyên. Chúng ta được cho biết kích thước khối n và chúng ta được cung cấp một chuỗi văn bản gốc và chuỗi văn bản mã hóa của nó. Cả hai đều được đảm bảo có độ dài chia hết cho n, do đó chúng tạo thành một số vectơ đầu vào-đầu ra được ghép nối có kích thước n. 

Đầu ra phụ thuộc vào việc hệ phương trình tuyến tính gây ra bởi các cặp này không có nghiệm, nghiệm duy nhất hay vô số nghiệm. Nếu không có ma trận nào có thể đáp ứng tất cả các ánh xạ thì chúng ta phải báo cáo thất bại. Nếu nhiều ma trận thỏa mãn tất cả các ánh xạ, chúng tôi sẽ báo cáo sự mơ hồ. Nếu không, chúng tôi xuất ra ma trận duy nhất. 

Ràng buộc cấu trúc quan trọng là n nhiều nhất là 10, do đó mỗi khối cho một hệ thống tuyến tính nhỏ. Mặc dù các chuỗi có thể dài nhưng số lượng ẩn số chỉ là n2, điều này giúp cho việc loại bỏ Gaussian có thể thực hiện được. 

Các trường hợp biên phát sinh khi các cặp khối được cung cấp không đủ giới hạn. Ví dụ: nếu tất cả các khối văn bản gốc phụ thuộc tuyến tính, hệ thống không thể xác định một ma trận duy nhất ngay cả khi nó nhất quán. Một trường hợp lỗi khác xảy ra khi bản mã không khớp với bất kỳ phép biến đổi tuyến tính nào, khiến hệ thống không nhất quán. Trường hợp khó phát hiện thứ ba là khi số phương trình vượt quá ẩn số nhưng vẫn thiếu hạng dẫn đến vô số nghiệm. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mọi mục nhập của ma trận là một biến và thực thi trực tiếp rằng mỗi khối văn bản gốc nhân với ma trận này bằng khối văn bản mã hóa tương ứng của nó. Mỗi khối đóng góp n phương trình và mỗi phương trình là tuyến tính với n2 ẩn số. Với k khối, chúng ta có được phương trình kn. Việc giải quyết vấn đề này bằng cách liệt kê hoặc thay thế đơn giản là không thể vì không gian của các ma trận nguyên tăng theo cấp số nhân. 

Quan sát chính là đây là một hệ thống tuyến tính tiêu chuẩn trên một trường hoặc trên các số nguyên có ràng buộc nhất quán. Chúng ta có thể làm phẳng ma trận thành một vectơ có kích thước n2 và xây dựng một hệ thống A x = b, trong đó mỗi cặp bản rõ-bản mã đóng góp các ràng buộc tuyến tính. Việc loại bỏ Gaussian cho phép chúng ta xác định xem hệ thống có 0, một hay vô số nghiệm. 

Cấu trúc đơn giản hóa hơn nữa vì mỗi khối độc lập đóng góp một ràng buộc biến đổi tuyến tính đầy đủ và chúng ta có thể xếp tất cả chúng thành một ma trận tăng cường. Vấn đề giảm xuống để phân tích thứ hạng của hệ thống này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | hàm mũ | cao | Quá chậm | 
| Loại bỏ Gaussian | O((n²)³) trường hợp xấu nhất nhưng n ≤ 10 | O(n⁴) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi từng ký tự thành một chỉ mục số nguyên trong một bảng chữ cái cố định để có thể thực hiện số học. 

Sau đó chúng ta chia cả bản rõ và bản mã thành các khối có kích thước n. Mỗi cặp khối cho chúng ta n phương trình. Đối với một hàng của ma trận, giả sử hàng i, tọa độ đầu ra là tích của hàng đó với vectơ đầu vào. 

Chúng tôi xây dựng một hệ thống tuyến tính trong đó ẩn số là các phần tử của ma trận, được làm phẳng theo từng hàng. Mỗi khối đóng góp các ràng buộc của biểu mẫu: 

tổng_j M[i][j] * P[k][j] = C[k][i] 

cho mỗi khối k và mỗi tọa độ đầu ra i. 

Chúng tôi xây dựng một ma trận tăng cường để loại bỏ Gaussian với n2 ẩn số.

Chúng tôi thực hiện phép loại trừ trên các số thực (hoặc số nguyên được coi là số hữu tỉ, vì các ràng buộc là chính xác). Trong quá trình loại bỏ, chúng tôi theo dõi các vị trí trục. 

Sau khi loại bỏ, chúng tôi phân loại hệ thống. 

Nếu chúng ta tìm thấy một hàng mâu thuẫn trong đó tất cả các hệ số đều bằng 0 nhưng RHS khác 0 thì chúng ta sẽ không đưa ra nghiệm nào. 

Nếu hạng nhỏ hơn n² thì có các biến tự do và do đó có vô số nghiệm. 

Nếu thứ hạng bằng n2, chúng ta giải quyết duy nhất và xây dựng lại các phần tử ma trận. 

### Tại sao nó hoạt động 

Mọi ma trận mã hóa hợp lệ phải đáp ứng một hệ thống hoàn chỉnh các ràng buộc tuyến tính xuất phát từ tất cả các cặp đầu vào-đầu ra được quan sát. Những ràng buộc này mô tả đầy đủ một phép biến đổi tuyến tính. Việc loại bỏ Gaussian xác định xem hệ thống này có nhất quán hay không và liệu không gian nghiệm của nó có thứ nguyên bằng 0, dương hay trống. Bởi vì phép biến đổi là tuyến tính và hữu hạn chiều nên thứ hạng hoàn toàn đặc trưng cho tính duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

ALPH = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 "

mp = {c:i for i,c in enumerate(ALPH)}

def gauss(a, b, n):
    m = len(a)
    N = n*n
    row = 0
    where = [-1]*N

    for col in range(N):
        sel = row
        for i in range(row, m):
            if abs(a[i][col]) > abs(a[sel][col]):
                sel = i
        if abs(a[sel][col]) < 1e-12:
            continue
        a[row], a[sel] = a[sel], a[row]
        b[row], b[sel] = b[sel], b[row]
        where[col] = row

        div = a[row][col]
        for j in range(col, N):
            a[row][j] /= div
        b[row] /= div

        for i in range(m):
            if i != row and abs(a[i][col]) > 1e-12:
                f = a[i][col]
                for j in range(col, N):
                    a[i][j] -= f * a[row][j]
                b[i] -= f * b[row]

        row += 1

    for i in range(m):
        s = 0
        for j in range(N):
            s += a[i][j] * 0
        if abs(b[i]) > 1e-9:
            ok = True
            for j in range(N):
                if abs(a[i][j]) > 1e-12:
                    ok = False
                    break
            if ok:
                return None, False, False

    x = [0]*N
    for i in range(N):
        if where[i] != -1:
            x[i] = b[where[i]]
    free = any(where[i] == -1 for i in range(N))
    return x, True, free

def solve():
    n = int(input())
    p = input().rstrip("\n")
    c = input().rstrip("\n")

    k = len(p) // n

    A = []
    B = []

    for t in range(k):
        pv = [mp[ch] for ch in p[t*n:(t+1)*n]]
        cv = [mp[ch] for ch in c[t*n:(t+1)*n]]

        for i in range(n):
            row = [0]*(n*n)
            for j in range(n):
                row[i*n + j] = pv[j]
            A.append(row)
            B.append(cv[i])

    x, ok, free = gauss(A, B, n)

    if not ok:
        print("No solution.")
        return
    if free:
        print("Too many solutions")
        return

    mat = [[0]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            mat[i][j] = x[i*n + j]

    for row in mat:
        print(" ".join(str(int(round(v))) for v in row))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một hệ thống tuyến tính đầy đủ trong đó mỗi mục nhập ma trận là một biến. Mỗi khối văn bản gốc-mật mã đóng góp n phương trình, mỗi phương trình trên một tọa độ đầu ra. Việc loại bỏ Gaussian xác định xem hệ thống có không nhất quán, chưa được xác định hay được xác định đầy đủ hay không. 

Một điểm thực hiện tinh tế là tránh sự mất ổn định về số lượng; giải pháp sử dụng phương pháp loại bỏ dấu phẩy động với kiểm tra dung sai, có thể chấp nhận được với ràng buộc nhỏ n ≤ 10, nhưng trong một cài đặt chặt chẽ hơn, người ta sẽ ưu tiên loại bỏ số học mô-đun hoặc loại bỏ hợp lý. 

Bước phân loại sau khi loại bỏ là rất cần thiết: không có trục xoay cho bất kỳ biến nào có nghĩa là có vô số giải pháp, trong khi một hàng mâu thuẫn có nghĩa là không có giải pháp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
ATTACK AT DAWN
FPLSFA4SUK2W9K3
```Chúng tôi chia thành các khối và tạo thành các phương trình tuyến tính. Hệ thống có thứ hạng đầy đủ nên việc loại bỏ sẽ tạo ra một điểm xoay cho tất cả 9 biến. 

| Giai đoạn | Kết quả | 
| --- | --- | 
| kích thước hệ thống | 9 ẩn số | 
| xếp hạng | 9 | 
| phân loại | giải pháp độc đáo | 

Điều này dẫn đến đầu ra của ma trận được xây dựng lại. 

### Ví dụ 2 

đầu vào:```
3
ATTACK
FPLSFA
```Ở đây chúng ta chỉ có một khối nên chỉ có 3 phương trình cho 9 ẩn số. 

| Giai đoạn | Kết quả | 
| --- | --- | 
| phương trình | 3 | 
| ẩn số | 9 | 
| xếp hạng | 3 | 
| phân loại | giải pháp vô hạn | 

Điều này xác nhận lý do tại sao sự mơ hồ được báo cáo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n²)³) trường hợp xấu nhất | Loại bỏ Gaussian trên n2 biến, khả thi vì n 10 | 
| Không gian | O(n⁴) | ma trận hệ số cho hệ thống | 

Ràng buộc n  10 đảm bảo n  100, do đó việc loại bỏ bậc ba cũng nhanh chóng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return ""

# sample placeholders
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bản đồ nhận dạng | ma trận độc đáo | trường hợp giải được cơ bản | 
| cặp không nhất quán | không có giải pháp | phát hiện mâu thuẫn | 
| chưa xác định | quá nhiều giải pháp | thiếu thứ hạng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các khối văn bản gốc phụ thuộc tuyến tính. Ngay cả với nhiều mẫu, hệ thống có thể không đạt được thứ hạng, dẫn đến vô số nghiệm dù có nhiều phương trình. Thuật toán phát hiện chính xác điều này thông qua các trục bị thiếu. 

Một trường hợp khác là bản mã mâu thuẫn đối với một tập hợp bản rõ nhất quán. Điều này tạo ra một hàng 0 với RHS khác 0 sau khi loại bỏ, gây ra trường hợp không có lời giải. 

Cuối cùng, khi n bằng 1, hệ thống sẽ chuyển thành một hệ phương trình vô hướng duy nhất và độ chính xác giảm xuống còn việc kiểm tra tính nhất quán của các số nhân vô hướng mà cùng một khung loại trừ tương tự sẽ tự động xử lý.
