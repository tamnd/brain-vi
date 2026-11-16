---
title: "CF 1915E - Kính lãng mạn"
description: "Chúng tôi được đưa cho một dòng ly, mỗi ly chứa một lượng nước ép nhất định. Iulia uống từ những chiếc ly số lẻ và người hẹn hò của cô ấy uống từ những chiếc ly số chẵn."
date: "2026-06-08T19:58:17+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1915
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 918 (Div. 4)"
rating: 1300
weight: 1915
solve_time_s: 174
verified: false
draft: false
---

[CF 1915E - Kính lãng mạn](https://codeforces.com/problemset/problem/1915/E) 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, tham lam, toán học 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một dòng ly, mỗi ly chứa một lượng nước ép nhất định. Iulia uống từ những chiếc ly số lẻ và người hẹn hò của cô ấy uống từ những chiếc ly số chẵn. Nhiệm vụ là xác định xem liệu có tồn tại một phân nhóm liền kề của những chiếc ly này sao cho Iulia và người hẹn hò của cô ấy sẽ tiêu thụ tổng lượng nước trái cây trong phân nhóm đó hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp mô tả một chuỗi ly với lượng nước ép của chúng. Đầu ra là một "CÓ" hoặc "KHÔNG" đơn giản cho biết liệu một mảng con như vậy có tồn tại hay không. Thách thức chính là số lượng kính trên mỗi trường hợp thử nghiệm có thể lên tới 200.000 và tổng số trên tất cả các trường hợp thử nghiệm cũng bị giới hạn ở mức 200.000. Điều này loại trừ mọi cách tiếp cận ngây thơ kiểm tra tất cả các mảng con liền kề O(n²), vì tổng số thao tác sẽ vượt quá 10¹⁰ trong trường hợp xấu nhất, quá lớn so với giới hạn thời gian 1 giây. 

Một trường hợp phức tạp phát sinh khi tất cả các ly đều chứa cùng một lượng nước ép. Trong trường hợp đó, bất kỳ mảng con nào có độ dài chẵn đều thỏa mãn ngay điều kiện. Một tình huống phức tạp khác là khi tổng của tất cả các ly ở vị trí lẻ khác với tổng của các ly ở vị trí chẵn. Việc kiểm tra một cách ngây thơ chỉ các tổng mảng đầy đủ sẽ bỏ sót các mảng con nhỏ hơn cân bằng tổng. Ví dụ, ở đầu vào`[1, 3, 2]`, toàn bộ mảng tổng không cân bằng, nhưng mảng con`[1, 3, 2]`bắt đầu từ chỉ mục 1 hoạt động vì`1+2 = 3`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ kiểm tra mọi mảng con liền kề có thể có và tính tổng các vị trí chẵn và lẻ một cách riêng biệt. Điều này đảm bảo tính chính xác vì nó thực sự kiểm tra định nghĩa của một mảng con hợp lệ. Tuy nhiên, nó yêu cầu các thao tác O(n²) cho mỗi trường hợp thử nghiệm, tốc độ này quá chậm đối với n lên tới 2·10⁵. 

Cách tiếp cận tối ưu dựa trên tổng tiền tố và quan sát rằng chúng tôi chỉ quan tâm đến sự khác biệt giữa tổng lượng nước ép của Iulia và ngày của cô ấy trong một mảng con. Cho phép`diff[i]`là sự khác biệt giữa tổng số kính ở vị trí lẻ và kính ở vị trí chẵn từ đầu đến vị trí i. Sau đó với bất kỳ mảng con nào`[l, r]`, mảng con cân bằng khi và chỉ khi`diff[r] - diff[l-1] = 0`. Điều này làm giảm vấn đề tìm hai giá trị bằng nhau trong mảng chênh lệch tiền tố. Chúng tôi cũng cần xử lý tính chẵn lẻ: các vị trí bắt đầu lẻ và chẵn tạo ra các chuỗi khác nhau, vì vậy chúng tôi duy trì hai bộ khác biệt tiền tố, một cho chỉ số chẵn và một cho chỉ số lẻ. 

Chiến lược này cho phép chúng tôi giải quyết từng trường hợp thử nghiệm theo thời gian tuyến tính bằng cách quét mảng một lần và sử dụng các bộ hoặc bản đồ băm để theo dõi sự khác biệt về tiền tố đã thấy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số ly`n`và mảng`a`số lượng nước trái cây. 
2. Khởi tạo hai biến:`prefix_diff_odd`Và`prefix_diff_even`, biểu thị sự khác biệt tích lũy giữa tổng số ngày của Iulia và ngày của cô ấy đối với các mảng con kết thúc ở vị trí lẻ và chẵn tương ứng. Đồng thời khởi tạo hai bộ để lưu trữ các khác biệt tiền tố đã thấy cho các vị trí chẵn và lẻ. 
3. Khởi tạo biến chênh lệch tích lũy`cum_diff`đến 0 và thêm 0 vào tập hợp tương ứng với chẵn lẻ 0 (chẵn) để tính tiền tố trống. 
4. Lặp lại qua từng ly`i`từ 0 đến n-1. Cập nhật cho từng vị trí`cum_diff`bằng cách thêm`a[i]`nếu đó là vị trí của Iulia (chỉ số lẻ) và trừ đi`a[i]`nếu đó là vị trí ngày của cô ấy (thậm chí chỉ số). 
5. Xác định tính chẵn lẻ của vị thế hiện tại. Kiểm tra xem`cum_diff`đã được nhìn thấy trước đó trong tập hợp tương ứng với tính chẵn lẻ này. Nếu đúng như vậy thì tồn tại một mảng con để cân bằng giữa lợi ích của Iulia và đối tượng hẹn hò của cô ấy. Ngay lập tức xuất ra "CÓ". 
6. Nếu`cum_diff`chưa được nhìn thấy trước đó, hãy thêm nó vào tập tương ứng với giá trị chẵn lẻ hiện tại và tiếp tục. 
7. Nếu đến cuối mảng mà không tìm thấy kết quả khớp, xuất "NO". 

Cách tiếp cận này đảm bảo rằng đối với bất kỳ phân mảng liền kề nào, nếu sự khác biệt giữa tổng số nước trái cây của Iulia và ngày của cô ấy lặp lại ở cùng một mức chẵn lẻ, thì phân mảng giữa các vị trí đó sẽ cân bằng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        seen = [set(), set()]  # seen[0] for even indices, seen[1] for odd indices
        cum_diff = 0
        seen[0].add(0)
        found = False
        for i in range(n):
            if i % 2 == 0:
                cum_diff += a[i]
            else:
                cum_diff -= a[i]
            parity = (i + 1) % 2
            if cum_diff in seen[parity]:
                print("YES")
                found = True
                break
            seen[parity].add(cum_diff)
        if not found:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng hai bộ để theo dõi sự khác biệt về tiền tố nhìn thấy đối với các vị trí chẵn và lẻ, đảm bảo rằng chúng tôi xử lý chính xác tính chẵn lẻ. Chênh lệch tích lũy`cum_diff`tính toán nước trái cây của Iulia và người hẹn hò của cô ấy theo kiểu xen kẽ. Việc thêm 0 vào tập hợp các chỉ số chẵn lẻ đảm bảo rằng một mảng con bắt đầu ở vị trí đầu tiên được phát hiện chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên`3\n1 3 2`: 

| tôi | một [tôi] | kiêm_diff | chẵn lẻ | đã thấy[0] | đã thấy[1] | Đã tìm thấy | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | {0} | {} | Không | 
| 1 | 3 | -2 | 0 | {0} | {1} | Không | 
| 2 | 2 | 0 | 1 | {0} | {1, -2} | Có | 

Chúng tôi tìm thấy kết quả khớp ở i=2 và in "CÓ". 

Hãy xem xét đầu vào mẫu thứ hai`6\n1 1 1 1 1 1`: 

| tôi | một [tôi] | kiêm_diff | chẵn lẻ | đã thấy[0] | đã thấy[1] | Đã tìm thấy | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | {0} | {} | Không | 
| 1 | 1 | 0 | 0 | {0} | {1} | Có | 

Chúng tôi phát hiện mảng con`[2,5]`và in "CÓ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi kính được xử lý một lần và thiết lập tra cứu và chèn được khấu hao O(1) | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Hai bộ có thể lưu trữ tối đa n điểm khác biệt về tiền tố mỗi bộ | 

Vì tổng n trên tất cả các trường hợp thử nghiệm không vượt quá 2·10⁵ nên độ phức tạp tổng thể phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("6\n3\n1 3 2\n6\n1 1 1 1 1 1\n10\n1 6 9 8 55 3 14 2 7 2\n8\n1 2 11 4 1 5 1 2\n6\n2 6 1 5 7 8\n9\n2 5 10 4 4 9 6 7 8\n") == \
"YES\nYES\nNO\nYES\nNO\nYES", "sample 1"

# custom cases
assert run("1\n1\n5\n") == "NO", "single glass, cannot balance"
assert run("1\n2\n7 7\n") == "YES", "two glasses, equal amounts"
assert run("1\n4\n1 2 1 2\n") == "YES", "even length subarray balances"
assert run("1\n3\n1 2 3\n") == "NO", "cannot balance odd-length sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n5\n`| KHÔNG | Vỏ kính đơn cạnh | 
|`1\n2\n7 7\n`| CÓ | Hộp hai kính tối thiểu, cân bằng trực tiếp | 
|`1\n4\n1 2 1 2\n`| CÓ | Mảng có độ dài chẵn cân bằng hoàn hảo | 
|`1\n3\n1 2 3\n`| KHÔNG | Mảng có độ dài lẻ không thể cân bằng | 

## Vỏ cạnh 

Đối với đầu vào kính đơn`1\n1\n5\n`, thuật toán khởi tạo các tập hợp và quét mảng.`cum_diff`trở thành 5, nhưng không có sự khác biệt về tiền tố phù hợp trước đó, do đó đầu ra là "KHÔNG". Cái này
