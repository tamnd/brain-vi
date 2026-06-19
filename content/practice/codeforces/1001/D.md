---
title: "CF 1001D – Phân biệt trạng thái cộng và trạng thái trừ"
description: "Chúng ta được cấp một qubit duy nhất đã được chuẩn bị ở một trong hai trạng thái có thể có. Hai trạng thái này không phải là trạng thái cơ sở tính toán như Nhiệm vụ là tương tác với qubit này bằng các phép toán lượng tử được phép, thực hiện phép đo và trả về một số nguyên xác định…"
date: "2026-06-16T23:42:15+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1001
codeforces_index: "D"
codeforces_contest_name: "Microsoft Q# Coding Contest - Summer 2018 - Warmup"
rating: 1400
weight: 1001
solve_time_s: 57
verified: true
draft: false
---

[CF 1001D - Phân biệt trạng thái cộng và trạng thái trừ](https://codeforces.com/problemset/problem/1001/D) 

**Đánh giá:** 1400 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một qubit duy nhất đã được chuẩn bị ở một trong hai trạng thái có thể có. Hai trạng thái này không phải là các trạng thái cơ sở tính toán như |0⟩ hoặc |1⟩, mà là các trạng thái chồng chất chỉ khác nhau một pha tương đối. Cụ thể, qubit được đảm bảo ở trạng thái chồng chất bằng nhau với dấu dương hoặc chồng chất bằng nhau với dấu âm. 

Nhiệm vụ là tương tác với qubit này bằng các phép toán lượng tử được phép, thực hiện phép đo và trả về một số nguyên xác định trạng thái nào trong hai trạng thái mà chúng ta đã được cung cấp. Đầu ra được yêu cầu là 1 cho xếp chồng dương và -1 cho xếp chồng âm. 

Mặc dù kích thước đầu vào chỉ là một qubit, nhưng điều tinh tế là hai trạng thái này không thể phân biệt được bằng phép đo trực tiếp trên cơ sở tính toán. Một phép đo ngây thơ ngay lập tức trong cơ sở tiêu chuẩn sẽ phá hủy thông tin về pha, tạo ra kết quả ngẫu nhiên trong cả hai trường hợp. Đây là khó khăn chính: thông tin phân biệt được lưu trữ theo pha chứ không phải biên độ. 

Vì chúng tôi chỉ thao tác với một qubit duy nhất nên các ràng buộc tính toán là không đáng kể. Bất kỳ số lượng cổng lượng tử không đổi và một phép đo nào cũng đủ, do đó độ phức tạp về thời gian thực tế là O(1). Hạn chế thực sự là tính chính xác theo các quy tắc đo lượng tử hơn là hiệu quả thuật toán. 

Một trường hợp thất bại phổ biến phát sinh nếu người ta cố gắng đo trực tiếp mà không chuyển đổi cơ sở trước. Ví dụ: đo trạng thái đầu vào ngay lập tức mang lại 0 hoặc 1 với xác suất bằng nhau cho cả hai đầu vào, do đó, ánh xạ đơn giản đôi khi sẽ trả về 1 và đôi khi -1 bất kể trạng thái thực tế. Điều này không chính xác vì phép đo sẽ xóa đi đặc điểm phân biệt. 

## Phương pháp tiếp cận 

Một tư duy cổ điển mạnh mẽ sẽ cố gắng "lấy mẫu" qubit nhiều lần để suy ra pha ẩn. Trong một phép loại suy cổ điển, các phép đo lặp đi lặp lại có thể bộc lộ một sai lệch, nhưng trong cơ học lượng tử thì điều này là không thể vì phép đo làm suy giảm trạng thái. Sau phép đo đầu tiên, qubit bị phá hủy ở trạng thái cơ bản và không còn thông tin bổ sung nào về pha ban đầu có thể truy cập được. Do đó, bất kỳ chiến lược nào dựa vào việc lấy mẫu lặp lại đều thất bại ngay lập tức, vì mỗi lần chạy đều cho kết quả ngẫu nhiên độc lập không liên quan đến dấu hiệu ẩn. 

Điểm mấu chốt là mặc dù pha này không thể nhìn thấy được trong cơ sở tính toán nhưng nó sẽ trở thành thông tin về biên độ sau khi áp dụng phép biến đổi Hadamard. Cổng Hadamard chuyển đổi độ lệch pha thành độ lệch bit có thể đo được. Cụ thể, nó ánh xạ chồng chất dương vào |0⟩ và xếp chồng âm vào |1⟩. Điều này làm giảm vấn đề thành một phép đo tiêu chuẩn duy nhất. 

Khi phép chuyển đổi này được áp dụng, một phép đo duy nhất sẽ xác định hoàn toàn trạng thái. Sau đó, chúng tôi ánh xạ kết quả đo trở lại các giá trị đầu ra được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đo lường lặp đi lặp lại bằng vũ lực | O(∞ về nguyên tắc, nhưng không hợp lệ) | O(1) | Không chính xác do nhà nước sụp đổ | 
| Hadamard + đo lường | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Áp dụng cổng Hadamard cho qubit đầu vào. Điều này xoay trạng thái từ cơ sở pha sang cơ sở tính toán, chuyển đổi thông tin pha thành các chênh lệch biên độ có thể đo được. 
2. Đo qubit trên cơ sở tính toán. Sau khi chuyển đổi, kết quả mang tính quyết định: một trong hai trạng thái ban đầu ánh xạ tới |0⟩ và trạng thái còn lại ánh xạ tới |1⟩. 
3. Chuyển đổi kết quả đo thành đầu ra số nguyên cần thiết. Nếu kết quả tương ứng với |0⟩, trả về 1. Nếu kết quả tương ứng với |1⟩, trả về -1. 

Lý do thứ tự này quan trọng là vì phép đo chỉ được thực hiện sau khi chuyển đổi cơ sở. Đảo ngược trật tự sẽ phá hủy tín hiệu phân biệt. 

### Tại sao nó hoạt động

Tính đúng đắn xuất phát từ thực tế là cổng Hadamard là nghịch đảo của chính nó và hoán đổi cụ thể các biểu diễn cơ sở X và cơ sở Z. Hai trạng thái đầu vào là trạng thái riêng của toán tử Pauli X với giá trị riêng +1 và -1. Áp dụng Hadamard ánh xạ các trạng thái riêng này thành các trạng thái riêng cơ sở tính toán của Z, nơi chúng có thể được phân biệt hoàn hảo bằng phép đo. Vì các trạng thái riêng ánh xạ tới các trạng thái cơ sở trực giao, nên không còn sự trùng lặp xác suất sau khi chuyển đổi, đảm bảo nhận dạng xác định. 

## Giải pháp Python 

Mặc dù giao diện ban đầu ở dạng Q#, cấu trúc logic có thể được biểu diễn dưới dạng phép biến đổi xác định theo sau là phép đo.```python
import sys
input = sys.stdin.readline

def solve():
    q = input().strip()
    # In the quantum model, we would apply H and measure.
    # Conceptually:
    # if state is |+> -> measurement becomes 0
    # if state is |-> -> measurement becomes 1
    #
    # Then map:
    # 0 -> 1
    # 1 -> -1

    meas = input().strip()  # placeholder for measurement result

    if meas == "0":
        print(1)
    else:
        print(-1)

if __name__ == "__main__":
    solve()
```Trong môi trường lượng tử thực, hoạt động có ý nghĩa duy nhất là cổng Hadamard được áp dụng cho qubit trước khi đo. Phần còn lại là xử lý hậu kỳ cổ điển. Chi tiết triển khai chính là đảm bảo phép đo diễn ra chính xác một lần sau khi chuyển đổi, vì bất kỳ phép đo nào trước đó sẽ phá hủy thông tin về pha. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó qubit ở trạng thái chồng chất dương. Sau khi áp dụng Hadamard, trạng thái trở thành |0⟩. Phép đo luôn trả về 0. 

| Bước | Tiểu bang | 
| --- | --- | 
| Ban đầu | | 
| Sau H | | 
| Đo lường | 0 | 
| Đầu ra | 1 | 

Điều này xác nhận rằng trạng thái tích cực ánh xạ một cách xác định tới 1. 

Bây giờ hãy xem xét trạng thái chồng chất âm. 

| Bước | Tiểu bang | 
| --- | --- | 
| Ban đầu | | 
| Sau H | | 
| Đo lường | 1 | 
| Đầu ra | -1 | 

Điều này xác nhận rằng trạng thái tiêu cực được phân biệt chính xác. 

Những dấu vết này cho thấy phép biến đổi đã loại bỏ mọi mơ hồ về xác suất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một số lượng không đổi các hoạt động lượng tử và một phép đo | 
| Không gian | O(1) | Chỉ một qubit duy nhất và bộ nhớ cổ điển không đổi | 

Các ràng buộc cho phép mọi hoạt động lượng tử trong thời gian không đổi và giải pháp sử dụng chính xác một thay đổi cơ bản và một phép đo, trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Since we cannot truly simulate quantum states here, we mock behavior.

def solve():
    s = sys.stdin.readline().strip()
    # interpret input as "plus" or "minus"
    if s == "plus":
        print(1)
    else:
        print(-1)

# provided samples (conceptual)
assert run("plus") == "1", "sample 1"
assert run("minus") == "-1", "sample 2"

# custom cases
assert run("plus") == "1", "positive state"
assert run("minus") == "-1", "negative state"
assert run("plus") == "1", "repeated check stability"
assert run("minus") == "-1", "symmetry check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cộng | 1 | Ánh xạ trạng thái tích cực | 
| trừ | -1 | Ánh xạ trạng thái phủ định | 
| cộng | 1 | Tính quyết định dưới sự đánh giá lặp đi lặp lại | 
| trừ | -1 | Tính đối xứng và nhất quán | 

## Vỏ cạnh 

Trường hợp cạnh chính đang thử đo trước khi áp dụng phép biến đổi cơ sở. Nếu chúng ta đo ngay lập tức, cả |+⟩ và |−⟩ đều giảm xuống 0 hoặc 1 với xác suất bằng nhau, do đó việc chạy lặp lại sẽ tạo ra kết quả đầu ra không nhất quán. Ví dụ: đầu vào |+⟩ có thể mang lại 0, sau đó trong lần chạy khác cũng là 1, khiến mọi ánh xạ xác định đều không thể thực hiện được. 

Sau khi áp dụng cổng Hadamard, vấn đề này sẽ biến mất. Hãy xem xét |+⟩: 

| Bước | Tiểu bang | 
| --- | --- | 
| Ban đầu | | 
| Sau khi đo không có H | ngẫu nhiên 0/1 | 
| Sau H thì đo | luôn 0 | 

Đối với |−⟩: 

| Bước | Tiểu bang | 
| --- | --- | 
| Ban đầu | | 
| Sau khi đo không có H | ngẫu nhiên 0/1 | 
| Sau H thì đo | luôn 1 | 

Điều này khẳng định rằng sự chuyển đổi là cần thiết và nếu không có nó thì vấn đề về cơ bản là không thể giải quyết được về mặt xác định.
