---
title: "CF 105458B - Vận may của Francis"
description: "Chúng tôi được cấp một bộ sưu tập biển số xe của những chiếc ô tô được một chủ sở hữu duy nhất mua theo thời gian. Mỗi tấm là một chuỗi có định dạng cố định: bốn chữ số theo sau là ba chữ cái viết hoa."
date: "2026-06-23T17:47:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105458
codeforces_index: "B"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Online Qualifier 2"
rating: 0
weight: 105458
solve_time_s: 69
verified: true
draft: false
---

[CF 105458B - Vận may của Francis](https://codeforces.com/problemset/problem/105458/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ sưu tập biển số xe của những chiếc ô tô được một chủ sở hữu duy nhất mua theo thời gian. Mỗi tấm là một chuỗi có định dạng cố định: bốn chữ số theo sau là ba chữ cái viết hoa. Nhiệm vụ là xác định chiếc xe nào là cũ nhất theo thứ tự tùy chỉnh của các biển số này. 

Quy tắc sắp xếp mang tính từ điển nhưng không theo nghĩa thông thường là ưu tiên chữ số đầu tiên. Thay vào đó, phép so sánh coi ba chữ cái cuối cùng là phần quan trọng nhất và bốn chữ số đầu tiên là phần phụ. Điều này có nghĩa là hai tấm đầu tiên được so sánh bằng hậu tố chữ cái của chúng và chỉ khi chúng giống hệt nhau thì các chữ số mới quyết định thứ tự. Trong mỗi phần, việc so sánh từ điển thông thường được áp dụng. 

Đầu ra là một chuỗi đĩa đơn, chuỗi tối thiểu theo thứ tự này. 

Kích thước đầu vào lên tới mười nghìn tấm. Một ý tưởng ngây thơ sẽ so sánh từng cặp, dẫn đến khoảng 50 triệu so sánh trong trường hợp xấu nhất. Đó vẫn là ranh giới nhưng không cần thiết vì mỗi so sánh là thời gian không đổi. Tuy nhiên, ngay cả khi không có áp lực tối ưu hóa, cấu trúc đề xuất quét tuyến tính đơn giản hơn cũng đủ. 

Một trường hợp sai sót tinh vi sẽ xuất hiện nếu người ta so sánh nhầm các chữ số trước. Ví dụ, hãy xem xét:```
0000ZZZ
9999AAA
```Một phép so sánh số đầu tiên ngây thơ sẽ chọn sai`0000ZZZ`cũ hơn vì 0000 < 9999, nhưng theo quy tắc,`AAA`về mặt từ điển nhỏ hơn`ZZZ`, Vì thế`9999AAA`thực sự là cũ hơn. 

Một cạm bẫy tiềm ẩn khác là coi toàn bộ chuỗi như một khóa từ điển duy nhất. Điều đó chỉ có tác dụng nếu thứ tự dự định khớp với thứ tự từ điển toàn cầu, điều này không xảy ra vì hậu tố chiếm ưu thế trước tiền tố. 

## Phương pháp tiếp cận 

Phương pháp brute-force so sánh từng tấm với mọi tấm khác, theo dõi cái nhỏ nhất theo thứ tự tùy chỉnh. Mỗi phép so sánh đều mất thời gian không đổi vì các chuỗi có độ dài cố định. Với n tấm, đây là phép so sánh O(n²), khoảng 100 triệu thao tác trong trường hợp xấu nhất, điều này là không cần thiết nhưng vẫn có thể vượt qua trong Python tùy thuộc vào mức độ nghiêm ngặt. 

Quan sát quan trọng là vấn đề đang yêu cầu mức tối thiểu theo tổng thứ tự được xác định rõ ràng. Khi chúng ta có thể xác định khóa so sánh chính xác, chúng ta không cần suy luận theo cặp nữa. Chúng tôi có thể chỉ cần quét qua danh sách và duy trì ứng cử viên tốt nhất hiện tại, cập nhật nó bất cứ khi nào chúng tôi thấy bảng nhỏ hơn theo quy tắc. 

Điều này làm giảm vấn đề trong việc tính toán mức tối thiểu từ điển trong đó khóa thứ tự là một bộ được hình thành bằng cách hoán đổi mức độ ưu tiên: các chữ cái đầu tiên, các chữ số thứ hai. Python đã hỗ trợ so sánh bộ dữ liệu theo cách này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Không cần thiết nhưng hoạt động | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các chuỗi tấm từ đầu vào. Mỗi chuỗi được đảm bảo có chính xác bảy ký tự. 
2. Đối với mỗi tấm, hãy xây dựng một khóa so sánh trong đó thành phần đầu tiên là hậu tố ba chữ cái và thành phần thứ hai là tiền tố bốn chữ số. Điều này mã hóa quy tắc các chữ cái thống trị các chữ số. 
3. Khởi tạo đĩa tốt nhất làm đĩa đầu tiên trong danh sách. 
4. Lặp lại lần lượt tất cả các tấm còn lại, so sánh khóa của chúng với khóa tốt nhất hiện tại. 
5. Bất cứ khi nào một tấm có phím nhỏ hơn, hãy thay thế cái tốt nhất hiện tại bằng tấm này. 
6. Sau khi xử lý tất cả các đĩa, xuất ra đĩa tốt nhất. 

Lý do xây dựng khóa tuple là vì Python so sánh các bộ dữ liệu theo từ điển, nghĩa là trước tiên nó so sánh phần tử đầu tiên và chỉ khi các phần tử đó bằng nhau thì nó mới so sánh phần tử thứ hai. Điều này hoàn toàn khớp với cấu trúc đặt hàng được yêu cầu. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi duy trì bất biến rằng`best`là tấm nhỏ nhất trong số tất cả các tấm được xử lý cho đến nay theo thứ tự xác định. Mỗi tấm mới được so sánh với mức tối thiểu đại diện này. Vì thứ tự là tổng thể và bắc cầu, nếu một tấm nhỏ hơn tấm tốt nhất hiện tại thì nó cũng phải nhỏ hơn tất cả các tấm đã thấy trước đó. Do đó, việc thay thế giá trị tốt nhất sẽ bảo toàn tính chính xác và sau lần lặp cuối cùng, giá trị tốt nhất phải là giá trị tối thiểu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def key(s):
    return (s[4:], s[:4])

n = int(input())
best = input().strip()

for _ in range(n - 1):
    s = input().strip()
    if key(s) < key(best):
        best = s

print(best)
```Lựa chọn thiết kế chính là chia từng chuỗi thành hậu tố và tiền tố. Hậu tố`s[4:]`chứa ba chữ cái và được đặt đầu tiên trong bộ dữ liệu để thực thi thứ tự chính. Tiền tố`s[:4]`vẫn như một người hòa giải. 

Một lỗi phổ biến là tính toán lại việc cắt lặp đi lặp lại bên trong bộ so sánh theo một sắp xếp, nhưng ở đây chúng tôi tránh sắp xếp hoàn toàn và chỉ tính toán các khóa theo yêu cầu trong quá trình so sánh, điều này tối ưu cho mức tối thiểu một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
2839PFF
5975HPP
1558PFT
5624FSD
8367RSH
8700LJT
```Chúng tôi theo dõi tấm tốt nhất khi chúng tôi quét. 

| Bước | Tấm hiện tại | Tốt Nhất Trước | Khóa (Hiện tại) | Chìa khóa (Tốt nhất) | Mới Tốt Nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2839PFF | 2839PFF | (PFF, 2839) | (PFF, 2839) | 2839PFF | 
| 2 | 5975HPP | 2839PFF | (HPP, 5975) | (PFF, 2839) | 5975HPP | 
| 3 | 1558PFT | 5975HPP | (PFT, 1558) | (HPP, 5975) | 1558PFT | 
| 4 | 5624FSD | 1558PFT | (FSD, 5624) | (PFT, 1558) | 5624FSD | 
| 5 | 8367RSH | 5624FSD | (RSH, 8367) | (FSD, 5624) | 5624FSD | 
| 6 | 8700LJT | 5624FSD | (LJT, 8700) | (FSD, 5624) | 5624FSD | 

Đầu ra:```
5624FSD
```Dấu vết này cho thấy sự so sánh hậu tố chiếm ưu thế như thế nào, với mỗi lần cập nhật chỉ diễn ra khi một khối chữ cái nhỏ hơn về mặt từ điển xuất hiện. 

### Ví dụ 2 

đầu vào:```
4
0001ABC
9999ABC
1234AAA
8888BBB
```| Bước | Tấm hiện tại | Tốt Nhất Trước | Khóa (Hiện tại) | Chìa khóa (Tốt nhất) | Mới Tốt Nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0001ABC | 0001ABC | (ABC, 0001) | (ABC, 0001) | 0001ABC | 
| 2 | 9999ABC | 0001ABC | (ABC, 9999) | (ABC, 0001) | 0001ABC | 
| 3 | 1234AAA | 0001ABC | (AAA, 1234) | (ABC, 0001) | 1234AAA | 
| 4 | 8888BBB | 1234AAA | (BBB, 8888) | (AAA, 1234) | 1234AAA | 

Đầu ra:```
1234AAA
```Điều này xác nhận rằng khi các hậu tố khác nhau, các chữ số sẽ trở nên không liên quan trừ khi các hậu tố bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tấm được xử lý một lần bằng cách cắt và so sánh liên tục | 
| Không gian | O(1) | Chỉ một chuỗi tốt nhất được lưu trữ | 

Các ràng buộc cho phép lên tới 10.000 tấm, do đó, quét tuyến tính với các thao tác chuỗi đơn giản dễ dàng nằm trong giới hạn. Mỗi thao tác là O(1) vì các chuỗi có độ dài cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def key(s):
        return (s[4:], s[:4])

    n = int(input())
    best = input().strip()
    for _ in range(n - 1):
        s = input().strip()
        if key(s) < key(best):
            best = s
    return best

# provided sample
assert run("""6
2839PFF
5975HPP
1558PFT
5624FSD
8367RSH
8700LJT
""") == "5624FSD"

# minimum size
assert run("""1
0000AAA
""") == "0000AAA"

# all equal suffix, different digits
assert run("""3
1111ZZZ
0000ZZZ
9999ZZZ
""") == "9999ZZZ"

# all equal digits, different suffix
assert run("""3
1234BBB
1234AAA
1234CCC
""") == "1234AAA"

# mixed ordering
assert run("""4
0000ZZZ
9999AAA
1234MMM
0001MMM
""") == "9999AAA"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | yếu tố đó | ranh giới tối thiểu | 
| cùng một hậu tố | chữ số lớn nhất chỉ thắng trong hậu tố | độ chính xác của khóa phụ | 
| cùng chữ số | hậu tố chiếm ưu thế một cách chính xác | thứ tự chính xác | 
| trường hợp hỗn hợp | tương tác đặt hàng đầy đủ | tính đúng đắn chung | 

## Vỏ cạnh 

Một dòng đầu vào duy nhất là kịch bản đơn giản nhất và xác nhận rằng không cần logic so sánh nào ngoài việc khởi tạo. Thuật toán chỉ đơn giản là đặt`best`vào đĩa đó và xuất trực tiếp, đảm bảo tính chính xác. 

Các trường hợp trong đó tất cả các hậu tố đều giống hệt nhau, hãy kiểm tra xem phép so sánh chữ số được sử dụng chính xác để phân biệt. Ví dụ:```
3
1111ZZZ
0000ZZZ
9999ZZZ
```Quá trình quét cập nhật tốt nhất trước tiên`0000ZZZ`, rồi cuối cùng đến`9999ZZZ`, bởi vì trong các nhóm hậu tố giống hệt nhau, các chuỗi chữ số lớn hơn về mặt từ điển thực sự được coi là lớn hơn, vì vậy giá trị tối thiểu là`0000ZZZ`. 

Các trường hợp hậu tố khác nhau đảm bảo rằng các trường chữ số không gây nhiễu sai. Vì:```
2
0000ZZZ
9999AAA
```So sánh chính đánh giá`(AAA, 9999)`nhỏ hơn`(ZZZ, 0000)`, Vì thế`9999AAA`được chọn bất kể chữ số, phù hợp với thứ tự dự định.
