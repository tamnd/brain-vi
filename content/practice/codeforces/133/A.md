---
title: "CF 133A - HQ9+"
description: "Nhiệm vụ là phân tích một chương trình được viết bằng ngôn ngữ bí truyền HQ9+ và xác định xem việc thực thi chương trình đó có tạo ra bất kỳ đầu ra hiển thị nào hay không. Chương trình được cung cấp dưới dạng một chuỗi bao gồm các ký tự ASCII có thể in được."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 133
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 96 (Div. 2)"
rating: 900
weight: 133
solve_time_s: 64
verified: true
draft: false
---

[CF 133A - HQ9+](https://codeforces.com/problemset/problem/133/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là phân tích một chương trình được viết bằng ngôn ngữ bí truyền HQ9+ và xác định xem việc thực thi chương trình đó có tạo ra bất kỳ đầu ra hiển thị nào hay không. Chương trình được cung cấp dưới dạng một chuỗi bao gồm các ký tự ASCII có thể in được. Trong HQ9+, chỉ có bốn ký tự có ý nghĩa hoạt động: "H" in "Xin chào, Thế giới!", "Q" in chính chương trình, "9" in lời bài hát "99 Chai Bia" và "+" sửa đổi bộ tích lũy bên trong nhưng không tạo ra đầu ra. Mọi ký tự khác đều bị bỏ qua. 

Từ góc độ thực thi, điều này có nghĩa là chương trình sẽ tạo ra đầu ra nếu nó chứa ít nhất một trong số "H", "Q" hoặc "9". Tất cả các ký tự khác, bao gồm "+", chữ cái, dấu chấm câu hoặc chữ số, không tạo ra kết quả hiển thị. Độ dài chuỗi nằm trong khoảng từ 1 đến 100, đủ nhỏ để mọi thuật toán kiểm tra từng ký tự riêng lẻ đều có thể chấp nhận được. Bởi vì đầu ra phụ thuộc vào sự hiện diện của các hướng dẫn cụ thể nên chúng tôi chỉ cần kiểm tra tư cách thành viên trên chuỗi chứ không cần bất kỳ mô phỏng thực tế nào về các hoạt động HQ9+. 

Các trường hợp cạnh bao gồm các chuỗi chỉ có hướng dẫn không xuất ra, chuỗi chứa nhiều hướng dẫn đầu ra và chuỗi có hướng dẫn đầu ra được nhúng giữa các ký tự không liên quan. Ví dụ, đầu vào`"Hi!"`chứa "H" và sẽ trả về`"YES"`, trong khi`"+++"`chỉ chứa "+" và sẽ trả về`"NO"`. Việc triển khai bất cẩn chỉ kiểm tra ký tự đầu tiên hoặc giả sử "+" tạo ra đầu ra sẽ không thành công trong những trường hợp này. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng toàn bộ chương trình, xử lý từng ký tự theo thứ tự và tạo ra các chuỗi đầu ra thực tế. Điều này hiệu quả vì nó sao chép một cách trung thực ngữ nghĩa của HQ9+, nhưng nó quá mức cần thiết cho yêu cầu của vấn đề. Trong trường hợp xấu nhất, chuỗi có thể chứa số "9", về mặt lý thuyết sẽ yêu cầu tạo ra lời bài hát đầy đủ của "99 Chai Bia". Điều đó là không cần thiết ở đây vì chúng ta chỉ cần một câu trả lời boolean chứ không phải văn bản in thực tế. 

Cách tiếp cận tối ưu thúc đẩy quan sát rằng chúng ta không cần bản thân đầu ra, chỉ cần nó có tồn tại hay không. Do đó, chúng ta có thể quét chuỗi chương trình một lần và kiểm tra xem nó có chứa bất kỳ chữ "H", "Q" hay "9" nào không. Nếu một nhân vật như vậy tồn tại, câu trả lời là`"YES"`, nếu không thì`"NO"`. Điều này làm giảm vấn đề xuống còn một lần truyền tuyến tính duy nhất trên chuỗi, tránh việc tạo chuỗi không cần thiết và đảm bảo giải pháp đơn giản, mạnh mẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Hoạt động nhưng nặng không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào đại diện cho chương trình HQ9+. Chúng tôi giả sử nó chỉ chứa các ký tự ASCII có thể in được và không trống. 
2. Khởi tạo một bộ chứa ba lệnh tạo đầu ra:`{'H', 'Q', '9'}`. Điều này cho phép kiểm tra tư cách thành viên liên tục. 
3. Lặp lại từng ký tự của chuỗi chương trình. 
4. Đối với mỗi ký tự, kiểm tra xem nó có tồn tại trong tập lệnh đầu ra hay không. 
5. Nếu trùng ký tự nào thì in ngay`"YES"`và chấm dứt, vì chúng ta đã tìm thấy ký tự tạo ra kết quả. 
6. Nếu vòng lặp hoàn tất mà không tìm thấy bất kỳ ký tự nào tạo ra kết quả đầu ra, hãy in`"NO"`. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán sẽ kiểm tra bộ ký tự chính xác đã biết để tạo ra kết quả hiển thị. Vì bất kỳ sự cố nào cũng đảm bảo kết quả đầu ra nên việc thoát ra sớm là an toàn. Ngược lại, nếu không có ký tự nào tồn tại trong chuỗi thì chương trình không thể tạo ra bất kỳ đầu ra nào, do đó trả về`"NO"`là đúng. Bất biến này giữ nguyên bất kể vị trí hoặc số lượng ký tự không liên quan khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

program = input().strip()
output_instructions = {'H', 'Q', '9'}

for ch in program:
    if ch in output_instructions:
        print("YES")
        break
else:
    print("NO")
```Đầu tiên, mã sẽ cắt bớt mọi dòng mới hoặc dấu cách ở cuối. bộ`output_instructions`cung cấp kiểm tra tư cách thành viên O(1). các`for`vòng lặp quét từng ký tự. các`else`điều khoản về`for`chỉ thực hiện nếu vòng lặp không bị ngắt, nghĩa là không tìm thấy lệnh tạo đầu ra. Đây là một tính năng Python tinh tế nhưng tiêu chuẩn giúp tránh cần thêm một biến cờ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`"Hi!"`| Bước | ch | ch trong đầu ra_instructions? | Hành động | 
| --- | --- | --- | --- | 
| 1 | 'H' | Có | In "CÓ", thoát | 
| - | - | - | Vòng lặp kết thúc sớm | 

Điều này xác nhận rằng chương trình sẽ phát hiện đầu ra ngay lập tức, ngay cả khi có thêm ký tự tiếp theo. 

**Đầu vào mẫu 2:**`"+++"`| Bước | ch | ch trong đầu ra_instructions? | Hành động | 
| --- | --- | --- | --- | 
| 1 | '+' | Không | Tiếp tục | 
| 2 | '+' | Không | Tiếp tục | 
| 3 | '+' | Không | Tiếp tục | 
| - | - | - | Vòng lặp hoàn tất, in "KHÔNG" | 

Điều này chứng tỏ việc xử lý đúng các chương trình không có hướng dẫn đầu ra rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét từng ký tự một lần và n ≤ 100 | 
| Không gian | O(1) | Tập hợp các hướng dẫn đầu ra có kích thước không đổi | 

Với những hạn chế, việc quét tuyến tính lên tới 100 ký tự là không đáng kể cả về thời gian và bộ nhớ. Giải pháp phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    program = sys.stdin.readline().strip()
    output_instructions = {'H', 'Q', '9'}
    for ch in program:
        if ch in output_instructions:
            return "YES"
    return "NO"

# Provided samples
assert run("Hi!\n") == "YES", "sample 1"
assert run("+++\n") == "NO", "sample 2"

# Custom cases
assert run("9\n") == "YES", "single '9'"
assert run("abcDEF\n") == "NO", "all non-instructions"
assert run("H+Q+9\n") == "YES", "multiple output instructions"
assert run("!\n") == "NO", "minimum ASCII non-instruction"
assert run("!H!Q!9!+\n") == "YES", "mixed characters with output"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "9" | "CÓ" | Lệnh đầu ra đơn | 
| "abcDEF" | "KHÔNG" | Tất cả các hướng dẫn không | 
| "H+Q+9" | "CÓ" | Nhiều hướng dẫn đầu ra | 
| "!" | "KHÔNG" | Không hướng dẫn ASCII tối thiểu | 
| "!H!Q!9!+" | "CÓ" | Hướng dẫn kết hợp không liên quan và đầu ra | 

## Vỏ cạnh 

chương trình`"+++"`kiểm tra một chuỗi chỉ chứa các lệnh không xuất ra. Thuật toán quét chính xác tất cả các ký tự, không tìm thấy ký tự nào trong`{'H','Q','9'}`, và bản in`"NO"`. đầu vào`"!H!Q!9!+"`chứa hỗn hợp các ký tự không liên quan và hướng dẫn đầu ra. Thuật toán phát hiện 'H' trên ký tự thứ hai, in`"YES"`ngay lập tức và thoát ra mà không quét phần còn lại. Những dấu vết này xác nhận tính chính xác của cả đường dẫn thoát sớm và đường dẫn quét toàn bộ.
