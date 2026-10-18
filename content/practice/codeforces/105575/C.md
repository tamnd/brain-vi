---
title: "CF 105575C - \u732b\u5a18\u9b54\u6cd5"
description: "Chúng ta được cung cấp một chuỗi duy nhất mã hóa một chuỗi các mã thông báo có kích thước cố định. Mỗi mã thông báo có ý nghĩa dài chính xác 5 ký tự và chuỗi phải được xem dưới dạng nối các khối 5 ký tự như vậy."
date: "2026-06-22T06:21:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105575
codeforces_index: "C"
codeforces_contest_name: "Southeast University 6th Programming Competition Competition (Winter, Novice Group) \u4e1c\u5357\u5927\u5b66\u7b2c\u516d\u5c4a\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u51ac\u5b63\u8d5b\uff08\u65b0\u624b\u7ec4\uff09"
rating: 0
weight: 105575
solve_time_s: 45
verified: true
draft: false
---

[CF 105575C - \u732b\u5a18\u9b54\u6cd5](https://codeforces.com/problemset/problem/105575/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất mã hóa một chuỗi các mã thông báo có kích thước cố định. Mỗi mã thông báo có ý nghĩa dài chính xác 5 ký tự và chuỗi phải được xem dưới dạng nối các khối 5 ký tự như vậy. 

Nhiệm vụ của chúng ta là quét các khối này từ trái sang phải và tìm khối đầu tiên khớp chính xác với mẫu`"moew~"`. Khi khối đó được tìm thấy, chúng tôi xuất vị trí của nó theo chuỗi khối, sử dụng chỉ mục dựa trên 1. 

Vì vậy, thay vì nghĩ về các ký tự riêng lẻ, sẽ tự nhiên hơn khi nghĩ đến chuỗi được chia thành các đoạn có kích thước 5. Chúng tôi đang tìm kiếm đoạn đầu tiên bằng một chuỗi mục tiêu cụ thể và báo cáo vị trí thứ tự của nó. 

Các ràng buộc được ngụ ý bởi cấu trúc này đủ nhỏ để chỉ cần quét tuyến tính trên các khối là đủ. Mỗi bước kiểm tra chính xác 5 ký tự, vì vậy ngay cả khi chuỗi lớn, tổng công việc vẫn tỷ lệ thuận với độ dài của nó. 

Một trường hợp lỗi nhỏ xuất hiện nếu ai đó quét từng ký tự thay vì nhảy theo bước 5. Ví dụ: nếu chuỗi là`"xxxxxmoew~yyyyy"`và chúng tôi kiểm tra mọi chuỗi con có độ dài 5 bắt đầu từ mọi chỉ mục, chúng tôi có thể vẫn đúng nhưng chúng tôi lãng phí công sức và có nguy cơ đưa ra các giả định sai lệch ngẫu nhiên. Việc giải thích đúng yêu cầu phải thực thi căn chỉnh theo bội số của 5 ngay từ đầu. 

Một trường hợp khác là giả sử trận đấu luôn bắt đầu ở chỉ số 0. Nếu khối đúng muộn hơn, giải pháp kiểm tra đơn giản sẽ ngay lập tức trả về kết quả sai. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp bằng vũ lực là kiểm tra mọi vị trí bắt đầu có thể`i`trong chuỗi và so sánh`s[i:i+5]`với`"moew~"`. Điều này đúng vì nó kiểm tra mọi chuỗi con ứng cử viên có độ dài phù hợp. Tuy nhiên, nó không hiệu quả về mặt cấu trúc vì nó kiểm tra các cửa sổ chồng chéo không phải là ranh giới khối hợp lệ. 

Trong trường hợp xấu nhất, điều này thực hiện khoảng`O(n)`so sánh chuỗi con và chi phí so sánh mỗi chuỗi con`O(5)`, vì vậy về tổng thể nó vẫn tuyến tính, nhưng nó bỏ qua cấu trúc mạnh hơn: các ứng cử viên hợp lệ chỉ xuất hiện ở các chỉ số chia hết cho 5. 

Quan sát quan trọng là chuỗi được phân chia theo khái niệm thành các đoạn không chồng chéo có độ dài 5. Khi chấp nhận điều này, chúng ta không còn cần phải xem xét mọi chỉ mục nữa. Chúng ta chỉ cần kiểm tra các vị trí`0, 5, 10, 15, ...`, giúp giảm vấn đề xuống còn quét khối đơn giản. Điều này loại bỏ sự dư thừa và làm cho mục đích của vấn đề khớp chính xác với việc triển khai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các chuỗi con) | O(n) | O(1) | Đã chấp nhận | 
| Quét khối (từng bước 5) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi chuỗi đầu vào là một chuỗi các mã thông báo có độ dài cố định và lặp lại chúng một cách trực tiếp. 

1. Bắt đầu từ ký tự đầu tiên của chuỗi. Vị trí này đại diện cho khối 5 ký tự đầu tiên. 
2. Trích xuất chuỗi con có độ dài 5 bắt đầu từ chỉ mục hiện tại. 
3. So sánh chuỗi con này với`"moew~"`. Nếu nó khớp, hãy tính số khối là`i // 5 + 1`và dừng lại. 
4. Nếu nó không khớp, hãy chuyển sang khối tiếp theo bằng cách tăng chỉ số lên 5. 
5. Lặp lại cho đến khi tìm thấy kết quả phù hợp. 

Lý do chúng tôi tăng thêm 5 mỗi lần là vì mọi ranh giới mã thông báo hợp lệ đều phải căn chỉnh theo bội số của 5. Việc kiểm tra các vị trí trung gian sẽ chỉ tạo ra các chuỗi con bị căn chỉnh sai và không có ý nghĩa trong cách xây dựng vấn đề. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ tính bất biến ở mỗi lần lặp, chỉ số`i`chính xác là ở đầu một khối. Vì tất cả các so sánh hợp lệ phải xảy ra ở ranh giới khối nên thuật toán không bao giờ bỏ qua một ứng cử viên hợp lệ. Đồng thời, nó không bao giờ đánh giá các giá trị bù trừ không hợp lệ, vì vậy mọi chuỗi con được kiểm tra đều tương ứng chính xác với một mã thông báo logic trong chuỗi đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    target = "moew~"
    
    i = 0
    while i < len(s):
        if s[i:i+5] == target:
            print(i // 5 + 1)
            return
        i += 5

if __name__ == "__main__":
    solve()
```Giải pháp đọc chuỗi đầu vào và lặp lại theo các bước cố định gồm 5 bước. Ở mỗi bước, giải pháp sẽ trích xuất chính xác một mã thông báo và so sánh nó với mẫu mục tiêu. Khi tìm thấy kết quả khớp, nó sẽ chuyển đổi chỉ mục ký tự thành chỉ mục khối bằng cách sử dụng phép chia số nguyên. 

Một lỗi phổ biến là lặp lại từng ký tự hoặc sử dụng vòng lặp như`for i in range(len(s) - 4)`, hoạt động nhưng mất đi ràng buộc về cấu trúc mà chỉ các vị trí căn chỉnh mới quan trọng. Kích thước bước 5 mã hóa trực tiếp cấu trúc dự định của vấn đề. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
aaaaamoew~xxxxx
```| tôi | s[i:i+5] | Trận đấu | Chặn | 
| --- | --- | --- | --- | 
| 0 | aaa | Không | - | 
| 5 | moe~ | Có | 2 | 

Tại`i = 5`, chúng tôi tìm thấy sự trùng khớp và đầu ra`5 // 5 + 1 = 2`. 

Điều này cho thấy thuật toán bỏ qua các chuỗi con bị sai lệch một cách chính xác và chỉ đánh giá các vị trí khối hợp lệ. 

Bây giờ hãy xem xét:```
moew~abcde
```| tôi | s[i:i+5] | Trận đấu | Chặn | 
| --- | --- | --- | --- | 
| 0 | moe~ | Có | 1 | 

Trận đấu diễn ra ngay ở khối đầu tiên, xác nhận việc xử lý đúng trường hợp tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự là một phần của chính xác một tấm séc dài 5 | 
| Không gian | O(1) | Chỉ một số biến được sử dụng ngoài bộ lưu trữ đầu vào | 

Thuật toán chạy thoải mái trong giới hạn vì mỗi lần lặp tăng thêm 5 ký tự, nghĩa là tổng số lần lặp là`n / 5`. Đây là tuyến tính và hiệu quả ngay cả đối với các chuỗi rất lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    from contextlib import redirect_stdout
    import io as sio
    
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    s = input().strip()
    target = "moew~"
    i = 0
    while i < len(s):
        if s[i:i+5] == target:
            print(i // 5 + 1)
            return
        i += 5

assert run("moew~") == "1"
assert run("aaaaamoew~xxxxx") == "2"
assert run("xxxxxmoew~") == "2"
assert run("moew~abcde") == "1"
assert run("aaaaabbbbbmoew~") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| moe~ | 1 | đầu vào tối thiểu | 
| aaaaamoew~xxxxx | 2 | trận đấu ở giữa | 
| xxxxxmoew~ | 2 | căn chỉnh sau offset | 
| moew~abcde | 1 | trận đấu ngay lập tức | 
| aaaabbbbmoew~ | 3 | vị trí sâu hơn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mục tiêu xuất hiện ngay từ đầu. Trong trường hợp này,`i = 0`và thuật toán ngay lập tức thành công mà không gặp bất kỳ vấn đề lặp lại nào. Chuỗi con`s[0:5]`trận đấu`"moew~"`, vì vậy đầu ra là`1`, điều đó đúng. 

Một trường hợp khác là khi trận đấu xuất hiện ở khối cuối cùng có thể. Vòng lặp tiếp tục bước từng bước 5 cho đến khi có chỉ số hợp lệ cuối cùng. Vì mỗi bước đều duy trì sự căn chỉnh nên khối cuối cùng vẫn được kiểm tra chính xác và không có truy cập ngoài giới hạn nào xảy ra do quá trình cắt trong Python được cắt ngắn một cách an toàn. 

Trường hợp thứ ba là khi độ dài chuỗi chính xác là 5. Thuật toán thực hiện một lần kiểm tra tại`i = 0`và chấm dứt ngay lập tức, xử lý chính xác kích thước đầu vào hợp lệ nhỏ nhất mà không cần xử lý đặc biệt.
