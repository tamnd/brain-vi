---
title: "CF 1722A - Kiểm tra chính tả"
description: "Chúng ta được cung cấp một chuỗi ngắn trong mỗi trường hợp thử nghiệm và chúng ta cần quyết định xem liệu nó có thể được hiểu là sự sắp xếp lại của tên “Timur” theo một quy tắc chính tả rất cụ thể hay không. Quy tắc không chỉ là các chữ cái phải khớp nhau."
date: "2026-06-15T01:24:50+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1722
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 817 (Div. 4)"
rating: 800
weight: 1722
solve_time_s: 122
verified: true
draft: false
---

[CF 1722A - Kiểm tra chính tả](https://codeforces.com/problemset/problem/1722/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn trong mỗi trường hợp thử nghiệm và chúng ta cần quyết định xem liệu nó có thể được hiểu là sự sắp xếp lại của tên “Timur” theo một quy tắc chính tả rất cụ thể hay không. 

Quy tắc không chỉ là các chữ cái phải khớp nhau. Chuỗi phải sử dụng chính xác năm chữ cái xuất hiện trong “Timur”, có cùng bội số và cấu trúc chữ hoa cũng phải khớp: chữ T phải xuất hiện chính xác một lần và phải viết hoa, trong khi các chữ cái còn lại phải là chữ cái i, m, u và r viết thường, mỗi chữ xuất hiện đúng một lần. 

Vì vậy, mỗi trường hợp kiểm thử về cơ bản là hỏi xem chuỗi đầu vào có phải là hoán vị hợp lệ của nhiều tập hợp {T, i, m, u, r} với ràng buộc kiểu chữ cố định hay không. 

Kích thước đầu vào cực kỳ nhỏ, tối đa 10 ký tự trên mỗi chuỗi và tối đa 1000 trường hợp kiểm thử. Điều này ngay lập tức ngụ ý rằng bất kỳ giải pháp nào chỉ cần quét hoặc so sánh trên mỗi lần kiểm tra đơn giản đều dễ dàng đủ nhanh. Không cần cấu trúc dữ liệu nâng cao hoặc tiền xử lý, vì ngay cả việc kiểm tra O(10) cho mỗi trường hợp kiểm thử cũng không đáng kể. 

Sự tinh tế chính là phân biệt chữ hoa chữ thường và kết hợp chính xác. Một vài lỗi phổ biến phát sinh ở đây. Người ta quên rằng chữ hoa rất quan trọng, vì vậy “timur” sẽ không thành công ngay cả khi các chữ cái khớp nhau. Một cách khác là cho phép các chữ cái lặp lại, chẳng hạn như “Timuur”, tạo ra sự trùng lặp và phá vỡ yêu cầu hoán vị. Thứ ba là chấp nhận các chuỗi có đúng chữ cái nhưng độ dài không chính xác, chẳng hạn như “Timr”, thiếu một ký tự và cần bị từ chối ngay lập tức. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là tạo ra tất cả các hoán vị của chuỗi “Timur” và kiểm tra xem chuỗi đầu vào có khớp với bất kỳ hoán vị nào trong số đó hay không. Vì tên có 5 ký tự riêng biệt nên có 5! = 120 hoán vị. Đối với mỗi trường hợp thử nghiệm, chúng tôi có thể so sánh với tất cả 120 ứng cử viên. Cách này có tác dụng nhưng không cần thiết và che giấu cấu trúc của vấn đề. 

Cấu trúc trở nên rõ ràng hơn nếu chúng ta nhận thấy rằng “chính tả hợp lệ” chỉ là thành viên trong một bộ năm chuỗi cố định. Mỗi câu trả lời hợp lệ phải chứa chính xác một trong mỗi ký tự bắt buộc có cách viết hoa cố định. Thay vì tạo ra các hoán vị, chúng ta có thể so sánh trực tiếp xem biểu diễn được sắp xếp hoặc tần số của đầu vào có khớp với mẫu tham chiếu hay không. 

Một cách nhìn đơn giản hơn là coi câu trả lời đúng là một tập hợp có nhiều ràng buộc. Chúng tôi kiểm tra xem độ dài có chính xác là 5 không, sau đó xác minh rằng mỗi ký tự bắt buộc xuất hiện chính xác một lần với kiểu chữ đúng. Điều này làm giảm vấn đề về việc kiểm tra liên tục theo thời gian cho mỗi trường hợp thử nghiệm. 

Cái nhìn sâu sắc quan trọng là chúng tôi không tìm kiếm sự sắp xếp; chúng tôi đang xác minh sự bình đẳng đối với một tập hợp cố định với các ràng buộc về chữ hoa chữ thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(120 · t) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Kiểm tra tần số / bộ trực tiếp | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định trước cấu trúc hợp lệ chính xác của tên dưới dạng bản đồ tần số: một 'T', một 'i', một 'm', một 'u' và một 'r'. Điều này hoạt động như chữ ký mục tiêu của chúng tôi. 
2. Đối với mỗi test case, trước tiên hãy kiểm tra độ dài của chuỗi. Nếu không phải là 5, chúng ta có thể từ chối ngay vì cách viết hợp lệ phải sử dụng đúng năm chữ cái. 
3. Xây dựng bộ đếm tần số cho các ký tự trong chuỗi đầu vào. Vì chuỗi rất nhỏ nên chỉ cần một từ điển đơn giản hoặc đếm trực tiếp là đủ. 
4. So sánh bản đồ tần số này với mục tiêu. Mỗi ký tự bắt buộc phải xuất hiện đúng một lần và không được phép thêm ký tự nào. 
5. Nếu tất cả các điều kiện đều phù hợp, xuất ra “CÓ”; nếu không thì xuất ra “NO”. 

Lý do thứ tự này quan trọng là tính hiệu quả và tính chính xác rõ ràng. Việc kiểm tra độ dài sẽ loại bỏ ngay nhiều trường hợp không hợp lệ và đảm bảo chúng tôi không vô tình chấp nhận các chuỗi có ký tự lặp lại vẫn chứa tất cả các chữ cái bắt buộc.

### Tại sao nó hoạt động 

Thuật toán dựa vào tính bất biến rằng cách viết hợp lệ của tên tương ứng chính xác với hoán vị của một tập hợp nhiều ký tự cố định. Hai chuỗi là hoán vị của nhau khi và chỉ khi chúng có chung tần số ký tự giống hệt nhau. Bởi vì yêu cầu phân biệt chữ hoa chữ thường được nhúng trực tiếp vào định nghĩa nhiều tập hợp, so sánh tần số nắm bắt đầy đủ mọi hạn chế của vấn đề. Không có thuộc tính cấu trúc nào khác của chuỗi quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

TARGET = {'T': 1, 'i': 1, 'm': 1, 'u': 1, 'r': 1}

def ok(s: str) -> bool:
    if len(s) != 5:
        return False
    cnt = {}
    for c in s:
        cnt[c] = cnt.get(c, 0) + 1
    return cnt == TARGET

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        print("YES" if ok(s) else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên sửa từ điển tần số dự kiến ​​cho tên hợp lệ. Mỗi trường hợp thử nghiệm được xử lý độc lập và chúng tôi bỏ qua độ dài được cung cấp`n`ngoại trừ việc kiểm tra tính nhất quán, vì độ dài chuỗi thực tế được đo trực tiếp. 

Việc so sánh tần số là chính xác nên bất kỳ sai lệch nào như viết sai, thiếu chữ cái hoặc ký tự lặp lại sẽ không thành công ngay lập tức. 

Một chi tiết triển khai tinh tế đang sử dụng`strip()`khi đọc chuỗi. Điều này ngăn các ký tự dòng mới can thiệp vào việc so sánh, đây là nguồn phổ biến gây ra lỗi ẩn trong cài đặt cuộc thi. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện từ mẫu. 

### Ví dụ 1 

Chuỗi đầu vào:`Timur`| Bước | Chuỗi | Kiểm tra độ dài | Bản đồ tần suất | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Timur | 5 | T:1 i:1 m:1 u:1 r:1 | CÓ | 

Điều này xác nhận trường hợp lý tưởng trong đó chuỗi khớp chính xác với nhiều tập hợp được yêu cầu. Mỗi ký tự xuất hiện một lần và viết hoa đúng. 

### Ví dụ 2 

Chuỗi đầu vào:`Timr`| Bước | Chuỗi | Kiểm tra độ dài | Bản đồ tần suất | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Timr | 4 | T:1 i:1 m:1 r:1 | KHÔNG | 

Ở đây việc kiểm tra độ dài không thành công ngay lập tức. Mặc dù tất cả các ký tự đều có vẻ đúng nhưng chữ 'u' bị thiếu sẽ làm mất hiệu lực yêu cầu hoán vị. 

Hai trường hợp này minh họa hai dạng lỗi cơ bản: độ dài không chính xác và thành phần tần số không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp kiểm thử xử lý tối đa 10 ký tự, vì vậy tổng công việc là tuyến tính theo số trường hợp kiểm thử | 
| Không gian | O(1) | Chỉ sử dụng từ điển tần số có kích thước cố định cho mỗi trường hợp thử nghiệm | 

Các ràng buộc đảm bảo rằng ngay cả việc triển khai trực tiếp nhất cũng dễ dàng nằm trong giới hạn. Giới hạn không đổi về độ dài chuỗi đảm bảo rằng việc đếm tần số thực sự là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(sys.stdin.readline())
    TARGET = {'T': 1, 'i': 1, 'm': 1, 'u': 1, 'r': 1}
    for _ in range(t):
        n = int(sys.stdin.readline())
        s = sys.stdin.readline().strip()
        cnt = {}
        if len(s) != 5:
            output.append("NO")
            continue
        for c in s:
            cnt[c] = cnt.get(c, 0) + 1
        output.append("YES" if cnt == TARGET else "NO")
    return "\n".join(output) + "\n"

# provided samples
assert run("""10
5
Timur
5
miurT
5
Trumi
5
mriTu
5
timur
4
Timr
6
Timuur
10
codeforces
10
TimurTimur
5
TIMUR
""") == """YES
YES
YES
YES
NO
NO
NO
NO
NO
NO
"""

# custom cases
assert run("""3
5
Timur
5
Turim
5
TiMur
""") == """YES
YES
NO
""", "case sensitivity check"

assert run("""2
5
Tiiur
5
Timurx
""") == """NO
NO
""", "duplicates and invalid char"

assert run("""1
1
T
""") == "NO\n", "minimum size invalid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hoán vị hợp lệ | CÓ | tính chính xác của tất cả các lần sắp xếp lại hợp lệ | 
| Trường hợp lỗi | KHÔNG | thực thi ràng buộc chữ hoa | 
| Ký tự không hợp lệ/trùng lặp | KHÔNG | kết hợp nhiều bộ nghiêm ngặt | 
| Kích thước tối thiểu | KHÔNG | xử lý ràng buộc độ dài | 

## Vỏ cạnh 

Một trường hợp thường gặp là khi có tất cả các chữ cái bắt buộc nhưng cách viết hoa lại sai, chẳng hạn như “timur”. Bản đồ tần số khớp về cấu trúc nhưng không khớp với các khóa chính xác, vì chữ 't' viết thường không phải là chữ hoa 'T' bắt buộc. Thuật toán từ chối chính xác điều này vì đẳng thức từ điển phân biệt chữ hoa chữ thường. 

Một trường hợp khác là các chữ cái được lặp lại như “Timuur”. Độ dài có thể vẫn là 5 nếu thiếu một chữ cái khác ở nơi khác, nhưng việc so sánh tần số sẽ ngay lập tức phát hiện ra sự mất cân bằng. Số lượng cho 'u' trở thành 2, không khớp chính xác với mục tiêu. 

Trường hợp cạnh cuối cùng là các chuỗi chứa các ký tự phụ như “Timurx”. Mặc dù năm chữ cái đầu tiên hợp lệ nhưng ký tự bổ sung sẽ phá vỡ cả điều kiện về độ dài và sự bằng nhau về tần số. Thuật toán sẽ loại bỏ nó ở giai đoạn kiểm tra độ dài, ngăn chặn việc tính toán không cần thiết.
