---
title: "CF 104013I - Bình phương số nguyên"
description: "Chúng ta có một năm duy nhất trong khoảng từ 1995 đến 2019. Mỗi năm có một người chiến thắng cuộc thi cố định (hoặc một cặp người chiến thắng trong một trường hợp đặc biệt) và nhiệm vụ là xuất ra chính xác chuỗi người chiến thắng tương ứng với năm nhất định."
date: "2026-07-02T05:04:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104013
codeforces_index: "I"
codeforces_contest_name: "2020-2021 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104013
solve_time_s: 128
verified: true
draft: false
---

[CF 104013I - Bình phương số nguyên](https://codeforces.com/problemset/problem/104013/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một năm duy nhất trong khoảng từ 1995 đến 2019. Mỗi năm có một người chiến thắng cuộc thi cố định (hoặc một cặp người chiến thắng trong một trường hợp đặc biệt) và nhiệm vụ là xuất ra chính xác chuỗi người chiến thắng tương ứng với năm nhất định. 

Đầu vào chỉ là một số nguyên, do đó toàn bộ vấn đề giảm xuống còn việc tra cứu từ năm đến giá trị được xác định trước. Không có tính toán nào ngoài việc chọn mục nhập chính xác. 

Vì kích thước đầu vào không đổi và rất nhỏ nên các ràng buộc về độ phức tạp không liên quan theo nghĩa thông thường. Mọi giải pháp từ kiểm tra có điều kiện trực tiếp đến lập chỉ mục mảng đều chạy ngay lập tức. Nguồn lỗi thực sự duy nhất là việc phiên âm không chính xác hoặc định dạng không khớp trong chuỗi đầu ra. 

Một trường hợp khó nhận thấy là năm 2006, khi có hai người chiến thắng cách nhau bằng dấu phẩy và dấu cách. Việc triển khai bất cẩn có thể phân chia chúng không chính xác, bỏ sót dấu phẩy hoặc thay đổi khoảng cách, điều này sẽ làm cho kết quả đầu ra không hợp lệ mặc dù về mặt logic vẫn đúng. 

Một vấn đề phổ biến khác là logic sai lệch khi sử dụng các mảng được lập chỉ mục từ 0 thay vì ánh xạ trực tiếp các năm, điều này có thể dễ dàng chuyển câu trả lời sang sai năm nếu không được căn chỉnh cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là lưu trữ danh sách tất cả các cặp (năm, người chiến thắng) và lặp lại tuyến tính để tìm năm phù hợp. Điều này đúng vì có chính xác một câu trả lời mỗi năm, nhưng nó vẫn thực hiện những công việc không cần thiết bằng cách quét tới 25 mục nhập cho mỗi truy vấn. 

Vì chỉ có một truy vấn và tập dữ liệu được cố định nên cải tiến tự nhiên là tính toán trước ánh xạ trực tiếp từ chuỗi năm tới chuỗi chiến thắng. Điều này có thể được thực hiện bằng cách sử dụng từ điển được khóa theo năm hoặc chênh lệch mảng vào năm 1995. Sau khi cấu trúc này được xây dựng, việc trả lời truy vấn chỉ là một tra cứu duy nhất. 

Quan sát quan trọng là tập dữ liệu tĩnh và nhỏ, do đó quá trình tiền xử lý chi phối mọi thứ và loại bỏ mọi nhu cầu tìm kiếm hoặc lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét tuyến tính qua danh sách |$O(25)$|$O(25)$| Đã chấp nhận | 
| Tra cứu trực tiếp (dict/array) |$O(1)$|$O(25)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ ánh xạ từ mỗi năm từ 1995 đến 2019 vào chuỗi người chiến thắng tương ứng. Điều này có thể được triển khai dưới dạng từ điển hoặc một mảng cố định được lập chỉ mục theo năm bù trừ, đảm bảo quyền truy cập liên tục. 
2. Đọc năm đầu vào$y$. 
3. Truy xuất giá trị liên quan đến$y$từ bản đồ. 
4. In chuỗi truy xuất chính xác như được lưu trữ, giữ nguyên dấu cách và dấu câu. 

### Tại sao nó hoạt động 

Mỗi năm trong phạm vi có chính xác một chuỗi đầu ra liên quan được xác định bởi câu lệnh vấn đề. Vì ánh xạ hoàn tất và có tính nội xạ trên miền đầu vào nên mọi đầu vào hợp lệ đều tương ứng với chính xác một giá trị được lưu trữ. Do đó, tra cứu trực tiếp sẽ tái tạo đầu ra được yêu cầu mà không có sự mơ hồ hoặc lỗi tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    y = int(input().strip())

    winners = {
        1995: "ITMO",
        1996: "SPbSU",
        1997: "SPbSU",
        1998: "ITMO",
        1999: "ITMO",
        2000: "SPbSU",
        2001: "ITMO",
        2002: "ITMO",
        2003: "ITMO",
        2004: "ITMO",
        2005: "ITMO",
        2006: "PetrSU, ITMO",
        2007: "SPbSU",
        2008: "SPbSU",
        2009: "ITMO",
        2010: "ITMO",
        2011: "ITMO",
        2012: "ITMO",
        2013: "SPbSU",
        2014: "ITMO",
        2015: "ITMO",
        2016: "ITMO",
        2017: "ITMO",
        2018: "SPbSU",
        2019: "ITMO"
    }

    print(winners[y])

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ toàn bộ ánh xạ một cách rõ ràng để việc tra cứu được thực hiện ngay lập tức. Chi tiết triển khai quan trọng duy nhất là giữ nguyên định dạng chính xác của mục nhập năm 2006, bao gồm dấu phẩy và dấu cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Năm đầu vào là 1995 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| 1 | Đọc đầu vào | 1995 | 
| 2 | Tra cứu bản đồ | ITMO | 
| 3 | Đầu ra | ITMO | 

Điều này xác nhận việc truy xuất trực tiếp cho năm đầu tiên trong tập dữ liệu. 

### Ví dụ 2 

Năm đầu vào là 2006 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| 1 | Đọc đầu vào | 2006 | 
| 2 | Tra cứu bản đồ | PetrSU, ITMO | 
| 3 | Đầu ra | PetrSU, ITMO | 

Điều này chứng tỏ rằng người chiến thắng tổng hợp phải được bảo toàn chính xác dưới dạng một chuỗi duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Tra cứu từ điển đơn | 
| Không gian |$O(1)$| Lập bản đồ kích thước không đổi trong 25 năm | 

Kích thước đầu vào không đổi, do đó giải pháp thỏa mãn tất cả các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    winners = {
        1995: "ITMO",
        1996: "SPbSU",
        1997: "SPbSU",
        1998: "ITMO",
        1999: "ITMO",
        2000: "SPbSU",
        2001: "ITMO",
        2002: "ITMO",
        2003: "ITMO",
        2004: "ITMO",
        2005: "ITMO",
        2006: "PetrSU, ITMO",
        2007: "SPbSU",
        2008: "SPbSU",
        2009: "ITMO",
        2010: "ITMO",
        2011: "ITMO",
        2012: "ITMO",
        2013: "SPbSU",
        2014: "ITMO",
        2015: "ITMO",
        2016: "ITMO",
        2017: "ITMO",
        2018: "SPbSU",
        2019: "ITMO"
    }

    return winners[int(inp.strip())]

assert run("1995") == "ITMO"
assert run("2006") == "PetrSU, ITMO"
assert run("2018") == "SPbSU"
assert run("2019") == "ITMO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1995 | ITMO | lập bản đồ năm sớm nhất | 
| 2006 | PetrSU, ITMO | định dạng nhiều người chiến thắng | 
| 2018 | SPbSU | tra cứu tầm trung | 
| 2019 | ITMO | năm ranh giới cuối cùng | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là năm đặc biệt có hai người chiến thắng, 2006. Kết quả đầu ra chính xác phải bao gồm cả hai tên được phân tách bằng dấu phẩy và dấu cách. Việc triển khai đúng sẽ coi giá trị này là giá trị chuỗi nguyên tử chứ không phải là một cặp có cấu trúc. 

Đối với đầu vào 2006, việc tra cứu trả về chính xác`"PetrSU, ITMO"`, và việc in nó không thay đổi sẽ giữ nguyên định dạng cần thiết. Bất kỳ chuyển đổi nào như phân tách bằng dấu phẩy hoặc cắt bớt khoảng trắng sẽ phá vỡ tính chính xác, mặc dù dữ liệu vẫn giữ nguyên về mặt ngữ nghĩa.
