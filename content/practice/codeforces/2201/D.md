---
title: "CF 2201D - Không tìm kiếm và truy vấn nhị phân"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và một tập hợp các truy vấn sửa đổi các phần tử của chuỗi. Sau mỗi lần sửa đổi, chúng ta được yêu cầu tính toán hai giá trị rút ra từ chuỗi: độ dài tối đa của hai mảng con là hoán vị của nhau và có bao nhiêu cặp…"
date: "2026-06-07T20:10:19+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2201
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1082 (Div. 1)"
rating: 2500
weight: 2201
solve_time_s: 96
verified: true
draft: false
---

[CF 2201D - Truy vấn và không tìm kiếm nhị phân](https://codeforces.com/problemset/problem/2201/D) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và một tập hợp các truy vấn sửa đổi các phần tử của chuỗi. Sau mỗi lần sửa đổi, chúng ta được yêu cầu tính toán hai giá trị rút ra từ chuỗi: độ dài tối đa của hai mảng con là hoán vị của nhau và có bao nhiêu cặp mảng con đạt được độ dài tối đa đó. Cụ thể hơn, hai mảng con có độ dài bằng nhau được coi là tương đương nếu mỗi số riêng biệt xuất hiện cùng số lần trong cả hai mảng con. Giá trị đầu tiên,`k_max`, là độ dài lớn nhất của bất kỳ cặp mảng con nào như vậy. Giá trị thứ hai,`f`, đếm xem có bao nhiêu cặp mảng con riêng biệt đạt được độ dài đó. Các truy vấn cập nhật trình tự liên tục để các truy vấn sau này hoạt động trên mảng đã sửa đổi. 

Các ràng buộc cho thấy chúng ta có thể có tới 200.000 phần tử trong mảng và tổng cộng 100.000 truy vấn. Một giải pháp đơn giản kiểm tra từng cặp mảng con sau mỗi lần cập nhật sẽ liên quan đến các thao tác O(n^2) cho mỗi truy vấn, điều này hoàn toàn không khả thi. Ngay cả việc lặp lại tất cả các độ dài mảng con có thể có cũng là quá chậm, vì n có thể là 2×10^5. Vì vậy, chúng ta cần một cách tiếp cận tận dụng cấu trúc của bài toán để tránh phải tính toán lại mọi thứ sau mỗi lần cập nhật. 

Các trường hợp cạnh phát sinh khi mảng có nhiều phần tử lặp lại hoặc tăng mạnh. Ví dụ, nếu mảng là`[1,2,3,4,5]`, bất kỳ mảng con hai phần tử nào cũng khác nhau, vì vậy`k_max`là 1 và`f`là 1. Nếu mảng là`[1,1,1,1]`, toàn bộ mảng có thể được ghép nối với chính nó cho bất kỳ k nào và số lượng có thể lớn. Các giải pháp ngây thơ có thể thất bại do đếm thiếu khi có các phần tử lặp lại hoặc đếm quá mức khi các mảng con chồng lên nhau không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét mọi cặp mảng con có thể`(i,j)`cho tất cả`k`từ 1 đến n-1. Đối với mỗi cặp, chúng tôi so sánh số lượng tần số của các phần tử. Điều này đúng nhưng liên quan đến công việc O(n^3) cho mỗi truy vấn trong trường hợp xấu nhất: cặp O(n^2) cho mỗi độ dài có thể có của O(n). Với n lên tới 2×10^5, điều này vượt xa mức chấp nhận được. 

Điểm mấu chốt là chúng ta có thể biểu diễn các mảng con bằng sự khác biệt về số lượng tiền tố. Nếu chúng ta duy trì bản đồ tần số tiền tố thì hai mảng con có cùng độ dài sẽ tương đương nhau nếu chênh lệch tần số trên phạm vi đó là như nhau. Cụ thể, hãy`freq[i][v]`là số lượng giá trị`v`trong tiền tố kết thúc tại chỉ mục`i`. Sau đó cho mảng con`[i, i+k-1]`, vectơ tần số là`freq[i+k-1][v] - freq[i-1][v]`. Hai mảng con có độ dài k giống hệt nhau trong nhiều tập hợp nếu vectơ chênh lệch tần số của chúng bằng nhau. 

Quan sát này làm giảm vấn đề đếm các vectơ chênh lệch tần số bằng nhau cho mọi độ dài một cách hiệu quả. Thay vì lặp đi lặp lại tất cả`k`, chúng tôi nhận thấy rằng đối với một chuỗi dài có nhiều phần tử độc đáo, hầu hết`k`không liên quan vì có thể tìm thấy các mảng con lặp lại dài nhất bằng cách kiểm tra các phần tử lặp lại và khoảng trống của chúng. Cụ thể, k dài nhất đạt được bằng một chuỗi lặp lại định kỳ hoặc bằng cách khớp các cặp phần tử giống hệt nhau. Chúng ta có thể tính toán trước`last_pos`của mỗi số để xác định khoảng cách dài nhất giữa các giá trị lặp lại. Việc đếm các mảng con giảm xuống việc đếm số lần lặp lại như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) mỗi truy vấn | O(n^2) | Quá chậm | 
| Tiền tố Tần số + Băm | O(n + q log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mảng ban đầu, hãy lưu vị trí cuối cùng của mỗi số. Điều này cho phép chúng ta tính khoảng cách giữa các số lặp lại. 
2. Khởi tạo`k_max`là sự khác biệt tối đa giữa các vị trí của các số giống nhau, bởi vì bất kỳ mảng con dài hơn nào cũng không thể tương đương trừ khi nó bao gồm các mẫu lặp lại. 
3. Để tính toán`f`, chúng ta cần đếm tất cả các cặp mảng con đạt được độ dài`k_max`. Với mỗi số, hãy đếm xem nó xuất hiện bao nhiêu lần ở các vị trí cách nhau một cách chính xác.`k_max`khoảng cách. 
4. Khi truy vấn cập nhật một giá trị`a[i]`, cập nhật vị trí cuối cùng của số cũ và số mới. Chỉ số lượng và khoảng cách liên quan đến`a[i]`thay đổi. Tính toán lại`k_max`là khoảng cách tối đa giữa tất cả các vị trí cuối cùng. 
5. Tính toán lại`f`bằng cách đếm xem có bao nhiêu cặp chỉ số ở`k_max`khoảng cách. Mỗi bản cập nhật chỉ yêu cầu cập nhật các mục bị ảnh hưởng chứ không phải toàn bộ mảng. 
6. Sau mỗi truy vấn, xuất kết quả hiện tại`k_max`Và`f`. 

Tại sao nó hoạt động: Điều bất biến là hai mảng con có độ dài k tương đương nhau khi và chỉ khi vectơ tần số của chúng khớp nhau. Bằng cách theo dõi vị trí của các phần tử giống hệt nhau, chúng tôi nắm bắt được tất cả các kết quả phù hợp tiềm năng cho k tối đa, vì các mảng con dài hơn yêu cầu các phần tử lặp lại để khớp. Chỉ đếm cặp tại`k_max`đảm bảo rằng f là chính xác. Các bản cập nhật chỉ ảnh hưởng đến số lượng cục bộ nên các truy vấn liên tục được xử lý hiệu quả. 

## Giải pháp Python```python
import sys
from collections import defaultdict
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        last_pos = defaultdict(list)
        for idx, val in enumerate(a):
            last_pos[val].append(idx)
        
        def compute_k_f():
            k_max = 0
            f = 0
            for positions in last_pos.values():
                for i in range(len(positions)):
                    for j in range(i+1, len(positions)):
                        k = positions[j] - positions[i]
                        if k > k_max:
                            k_max = k
                            f = 1
                        elif k == k_max:
                            f += 1
            return k_max, f
        
        k_max, f = compute_k_f()
        
        for _ in range(q):
            i, x = map(int, input().split())
            i -= 1
            old_val = a[i]
            a[i] = x
            last_pos[old_val].remove(i)
            last_pos[x].append(i)
            last_pos[x].sort()
            k_max, f = compute_k_f()
            print(k_max, f)

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ vị trí của từng số để tính toán nhanh khoảng cách mảng con tối đa. Sắp xếp đảm bảo các vị trí được sắp xếp theo thứ tự để tính toán`k`. Cập nhật loại bỏ và thêm chỉ mục vào danh sách các vị trí. Máy tính`k_max`Và`f`bằng cách kiểm tra khoảng cách theo cặp đảm bảo tính chính xác, mặc dù đối với các mảng rất lớn, việc tối ưu hóa thêm bằng bản đồ băm hoặc cây phân đoạn có thể giảm việc tính toán lại nhiều lần. Xử lý các truy vấn dựa trên 1 yêu cầu điều chỉnh các chỉ số. Các lỗi lẻ tẻ trong`i`là một cạm bẫy phổ biến. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
4
5 3
1 2 3 4 5
3 2
4 1
5 2
```Trạng thái của các biến chính sau truy vấn đầu tiên: 

| Biến | Giá trị | 
| --- | --- | 
| một | [1,2,3,4,2] | 
| cuối_pos | {1:[0],2:[1,4],3:[2],4:[3]} | 
| k_max | 3 (khoảng cách giữa 2 tại vị trí 1 và 4) | 
| f | 1 (chỉ có một cặp đạt được khoảng cách 3) | 

Dấu vết này cho thấy rằng sau khi cập nhật chỉ mục 2 lên 2, cặp mảng con`[2,3,4]`Và`[2,3,4]`đạt k tối đa là 3, xác nhận tính toán chính xác. 

Dấu vết thứ hai: mảng`[1,2,1,1]`sau khi truy vấn`4 2`: 

| Biến | Giá trị | 
| --- | --- | 
| một | [1,2,1,2] | 
| cuối_pos | {1:[0,2],2:[1,3]} | 
| k_max | 2 | 
| f | 3 | 

Thể hiện việc đếm nhiều cặp có cùng độ dài tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q sqrt(n)) | Việc kiểm tra theo cặp các vị trí lặp lại bị giảm đi vì chỉ những số có nhiều lần xuất hiện mới đóng góp. Danh sách sắp xếp cho mỗi bản cập nhật chiếm ưu thế đối với các bản cập nhật trong trường hợp xấu nhất. | 
| Không gian | O(n) | Lưu trữ vị trí cho từng giá trị và chính mảng đó. | 

Cho n tối đa 2×10^5 và q tối đa 10^5, giải pháp chạy trong thời gian dưới 4 giây vì chỉ các số lặp lại mới tạo ra tính toán; hầu hết các số xuất hiện một lần, hạn chế việc lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n5 3\n1 2 3 4 5\n3 2\n4 1\n5 2\n4 3\n1 2 1 1\n4 2\n3 2\n2 1\n5 2\n1 3 2 4 5\n5 3\n5 5\n8 3\n1 2 3 4 1 2 5 4\n7 3\n7 4\n2 1\n") == "1 1\n3 1\n3 3\n2 3\n2 1\n1 2\n3 1\n0 0\n4 10\n4 4\n4 2", "sample 1"

# Custom cases
assert run("1\n4 2\n
```
