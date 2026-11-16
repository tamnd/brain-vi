---
title: "CF 188E - HQ9+"
description: "Chúng ta được yêu cầu phân tích một chuỗi biểu diễn chương trình được viết bằng HQ9+, một ngôn ngữ đồ chơi với bốn hướng dẫn: H, Q, 9 và +. Chỉ có ba kết quả đầu tiên tạo ra khi được thực thi: H in \"Xin chào, Thế giới!"
date: "2026-06-03T01:06:39+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 188
codeforces_index: "E"
codeforces_contest_name: "Surprise Language Round 6"
rating: 1400
weight: 188
solve_time_s: 57
verified: true
draft: false
---

[CF 188E - HQ9+](https://codeforces.com/problemset/problem/188/E) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu phân tích một chuỗi đại diện cho một chương trình được viết bằng HQ9+, một ngôn ngữ đồ chơi có bốn hướng dẫn:`H`,`Q`,`9`, Và`+`. Chỉ có ba đầu ra đầu tiên tạo ra khi được thực thi:`H`in "Xin chào thế giới!",`Q`in chính chương trình đó và`9`in lời bài hát "99 Chai Bia". các`+`lệnh sửa đổi bộ tích lũy bên trong, nhưng không in ra bất cứ thứ gì. Bất kỳ ký tự nào khác trong chuỗi đầu vào đều bị bỏ qua. 

Đầu vào là một dòng chứa từ 1 đến 100 ký tự ASCII giữa`!`(33) và`~`(126). Nhiệm vụ của chúng ta là xác định xem việc thực hiện chương trình này có tạo ra kết quả nào hay không. Đầu ra là "CÓ" đơn giản nếu có ít nhất một lệnh in nội dung nào đó và "KHÔNG" nếu ngược lại. 

Vấn đề được hạn chế đủ để việc quét tuyến tính của chuỗi là khả thi. Ngay cả ở độ dài tối đa 100 ký tự, việc lặp qua từng ký tự để kiểm tra xem nó có đúng không`H`,`Q`, hoặc`9`sẽ yêu cầu tối đa 100 thao tác, không đáng kể trong giới hạn thời gian 2 giây. 

Trường hợp cạnh là một chuỗi bao gồm toàn bộ`+`ký tự hoặc các ký tự không xuất ra khác. Ví dụ,`"+++"`sẽ tạo ra "KHÔNG", trong khi một chuỗi có`"H++"`nên tạo ra "CÓ" vì`H`. Một trường hợp cạnh khác là một chuỗi trong đó lệnh tạo đầu ra xuất hiện ở cuối, ví dụ:`"abc9"`. Việc triển khai bất cẩn có thể trả về "KHÔNG" nếu dừng quét quá sớm hoặc không kiểm tra chính xác tất cả các ký tự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng chương trình: lặp qua từng ký tự và thực hiện hành động tương ứng. Điều này sẽ liên quan đến việc in hoặc đếm thực tế, nhưng vì chúng ta chỉ quan tâm đến việc đầu ra có xuất hiện hay không nên việc mô phỏng đầy đủ là không cần thiết. Brute-force hoạt động vì ngôn ngữ chỉ có bốn hướng dẫn, nhưng về mặt khái niệm thì nó quá mức cần thiết vì thông tin duy nhất cần thiết là sự hiện diện của`H`,`Q`, hoặc`9`. 

Cách tiếp cận tối ưu thúc đẩy quan sát rằng chúng ta không cần thực hiện chương trình. Đầu ra xảy ra khi và chỉ khi có ít nhất một ký tự trong chuỗi là một trong`H`,`Q`, hoặc`9`. Điều này làm giảm vấn đề thành một kiểm tra tư cách thành viên đơn giản: lặp qua chuỗi và ngay lập tức trả về "CÓ" khi gặp bất kỳ ký tự nào trong ba ký tự, chỉ trả về "KHÔNG" nếu không tìm thấy ký tự nào. Cách tiếp cận này là tuyến tính theo độ dài của đầu vào và không yêu cầu bất kỳ bộ nhớ bổ sung nào ngoài một vài biến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Quá mức cần thiết nhưng đúng | 
| Kiểm tra sự hiện diện đầu ra | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào và loại bỏ bất kỳ dòng mới nào ở cuối. Điều này đảm bảo chúng tôi chỉ xử lý chính mã chương trình. 
2. Lặp lại từng ký tự trong chuỗi. Ở mỗi bước, hãy kiểm tra xem ký tự đó có`H`,`Q`, hoặc`9`. Đây là những hướng dẫn duy nhất tạo ra đầu ra. 
3. Nếu ký tự trùng khớp, in ngay "CÓ" và kết thúc. Không cần phải tiếp tục quét vì một lệnh tạo đầu ra đảm bảo rằng chương trình sẽ tạo ra đầu ra. 
4. Nếu vòng lặp kết thúc mà không gặp bất kỳ ký tự nào trong ba ký tự, hãy in "NO". Điều này ghi lại trường hợp chương trình không có hướng dẫn tạo đầu ra. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng nếu đầu ra tồn tại trong chương trình, nó sẽ được phát hiện khi lệnh tạo đầu ra xuất hiện lần đầu tiên. Vì tất cả các lệnh tạo đầu ra đều được biết rõ ràng và phân biệt chữ hoa chữ thường, nên không có đầu ra nào có thể xảy ra từ các ký tự khác, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

program = input().strip()

for char in program:
    if char in "HQ9":
        print("YES")
        break
else:
    print("NO")
```Mã đọc chuỗi chương trình và lặp qua từng ký tự. các`in`kiểm tra xác định hướng dẫn sản xuất đầu ra. các`break`đảm bảo chúng tôi chấm dứt ngay khi phát hiện đầu ra. các`else`điều khoản của`for`vòng lặp chỉ được thực thi nếu vòng lặp hoàn thành mà không gặp phải`break`, cho phép chúng tôi xử lý trường hợp "KHÔNG đầu ra" một cách rõ ràng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
Hello!
```| Bước | Nhân vật | Kiểm tra | Hành động | 
| --- | --- | --- | --- | 
| 1 | H | H trong "HQ9" | In "CÓ" và ngắt | 

Ký tự đầu tiên là`H`, tạo ra đầu ra. Thuật toán kết thúc ngay lập tức, chứng tỏ rằng việc kết thúc sớm hoạt động chính xác. 

Đầu vào mẫu 2:```
+++
```| Bước | Nhân vật | Kiểm tra | Hành động | 
| --- | --- | --- | --- | 
| 1 | + | Không có trong "HQ9" | Tiếp tục | 
| 2 | + | Không có trong "HQ9" | Tiếp tục | 
| 3 | + | Không có trong "HQ9" | Tiếp tục | 
| Vòng lặp kết thúc | - | - | In "KHÔNG" | 

Không có ký tự nào tạo ra đầu ra, xác nhận thuật toán xác định chính xác các chương trình có đầu ra bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự của chuỗi được kiểm tra nhiều nhất một lần; n 100 | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng; không có cấu trúc dữ liệu bổ sung | 

Với n ≤ 100, giải pháp này chạy trong thời gian không đáng kể và sử dụng bộ nhớ tối thiểu, trong giới hạn 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    program = input().strip()
    for char in program:
        if char in "HQ9":
            return "YES"
    return "NO"

# Provided samples
assert run("Hello!\n") == "YES", "sample 1"
assert run("+++\n") == "NO", "sample 2"

# Custom cases
assert run("Q\n") == "YES", "single output instruction"
assert run("abcdefg\n") == "NO", "no instructions"
assert run("abcHxyz\n") == "YES", "output instruction in middle"
assert run("99Bottles\n") == "YES", "contains 9 producing output"
assert run("++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++\n") == "NO", "maximum-size non-output"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Q | CÓ | Lệnh đầu ra đơn | 
| abcdefg | KHÔNG | Không có hướng dẫn đầu ra | 
| abcHxyz | CÓ | Hướng dẫn đầu ra ở giữa | 
| 99Chai | CÓ | Chứa 9 | 
| +++++... (tối đa 100) | KHÔNG | Đầu vào không có đầu ra kích thước tối đa | 

## Vỏ cạnh 

Hãy xem xét một chương trình bao gồm hoàn toàn`+`nhân vật:`"++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++"`. Vòng lặp kiểm tra từng ký tự, không tìm thấy ký tự nào`H`,`Q`, hoặc`9`, và`else`mệnh đề in "KHÔNG". Điều này xác nhận rằng thuật toán xử lý chính xác các chuỗi chỉ có các hướng dẫn không đầu ra. 

Đối với một chương trình có một`9`ở phần cuối:`"abc9"`, vòng lặp lặp đi lặp lại`a`,`b`,`c`không có hành động, đạt tới`9`, in "CÓ" và thoát ngay lập tức. Điều này cho thấy rằng đầu ra được phát hiện muộn trong chuỗi được xử lý chính xác và việc chấm dứt sớm không tạo ra kết quả âm tính giả.
