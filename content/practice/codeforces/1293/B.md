---
title: "CF 1293B - JOE có trên TV!"
description: "Trò chơi có thể được coi là một quá trình trong đó chúng ta bắt đầu với n đối thủ và liên tục kích hoạt các vòng để loại bỏ một số đối thủ trong số đó."
date: "2026-06-16T04:35:47+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1293
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 614 (Div. 2)"
rating: 1000
weight: 1293
solve_time_s: 565
verified: false
draft: false
---

[CF 1293B - JOE có trên TV!](https://codeforces.com/problemset/problem/1293/B) 

**Đánh giá:** 1000 
**Tags:** tổ hợp, tham lam, toán học 
**Thời gian giải:** 9m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi có thể được coi là một quá trình mà chúng ta bắt đầu với`n`đối thủ và liên tục kích hoạt các hiệp đấu để loại bỏ một số đối thủ. Trong mỗi vòng, nếu có`s`đối thủ vẫn còn trong trò chơi và chính xác`t`trong số họ trượt câu hỏi hiện tại, JOE kiếm được`t/s`đô la và những thứ đó`t`người chơi bị loại sẽ bị xóa vĩnh viễn. Phần còn lại`s - t`tiếp tục vào vòng tiếp theo. 

Quyền tự do chính là chúng ta có thể quyết định có bao nhiêu đối thủ thất bại trong mỗi hiệp, miễn là nó nằm trong khoảng`0`và số lượng người sống sót hiện tại. Quá trình kết thúc khi không còn đối thủ. 

Mục tiêu là chọn một chuỗi loại bỏ sao cho tối đa hóa tổng số tiền mà JOE kiếm được. 

Ràng buộc`n ≤ 100000`có nghĩa là bất kỳ giải pháp nào cố gắng mô phỏng tất cả các cách có thể để phân bổ loại bỏ qua các vòng đều không khả thi. Một tìm kiếm ngây thơ về sự lựa chọn của`t`tại mỗi tiểu bang có nhiều nhánh và nhanh chóng trở thành cấp số nhân. Ngay cả việc lập trình động bậc hai trên tất cả các trạng thái còn lại có thể có cũng sẽ quá chậm. 

Đầu ra là một số thực, vì vậy chúng ta cũng cần một cách ổn định để tính tổng có thể lên đến`n`các thuật ngữ phân số. 

Một số tình huống cận biên quan trọng đối với tính đúng đắn. Khi`n = 1`, chỉ có một đối thủ và hành động có ý nghĩa duy nhất là loại bỏ họ trong một hiệp duy nhất, trao phần thưởng`1/1 = 1`. Bất kỳ lý do nào giả định nhiều vòng hoặc chia không chính xác sẽ bị phá vỡ ở đây. Một trường hợp thất bại tinh vi khác xuất hiện khi người ta cho rằng việc loại bỏ nhiều đối thủ cùng một lúc là có lợi. Ví dụ, lấy tất cả`n`ngay lập tức mang lại phần thưởng`n/n = 1`, có vẻ tốt ở địa phương nhưng hoàn toàn bỏ qua các vòng trong tương lai. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực xử lý từng trạng thái`s`như một sự lựa chọn: chọn bất kỳ`t`từ`1`ĐẾN`s`, nhận được`t/s`, và chuyển đến`s - t`. Điều này tạo thành một cây quyết định. Mặc dù đúng về nguyên tắc, nhưng nó bùng nổ vì mỗi trạng thái phân nhánh thành`s`khả năng, dẫn đến một số lượng lớn các trình tự ngay cả đối với mức độ vừa phải`n`. 

Sự đơn giản hóa chính đến từ việc so sánh cách tích lũy phần thưởng khi chúng tôi chia các lần loại trừ thành nhiều vòng so với việc nhóm chúng thành một. Giả sử ở một trạng thái nào đó`s`chúng tôi loại bỏ`k`đối thủ. Làm nó trong một vòng sẽ mang lại`k/s`. Thay vào đó, nếu chúng ta loại bỏ từng người một trong các vòng riêng biệt thì phần đóng góp sẽ trở thành`1/s + 1/(s-1) + ... + 1/(s-k+1)`, lớn hơn hẳn vì mỗi mẫu số giảm trong khi tử số giữ nguyên`1`. 

Điều này có nghĩa là việc tập trung loại bỏ thành một đợt lớn luôn tệ hơn việc chia chúng thành các đợt loại bỏ riêng lẻ. Vì vậy, chiến lược tối ưu là bắt buộc: trong mỗi hiệp đấu chúng ta loại bỏ chính xác một đối thủ. 

Khi điều đó đã được khắc phục, quá trình sẽ trở nên mang tính quyết định. Bắt đầu từ`n`, chúng tôi đi đến`n-1`, sau đó`n-2`, v.v. cho đến khi`1`, tích lũy một tổng đơn giản nghịch đảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về các lựa chọn loại bỏ | Hàm mũ | O(n) đệ quy | Quá chậm | 
| Chiến lược điều hòa tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

1. Bắt đầu với`s = n`và tổng phần thưởng`ans = 0`. Điều này thể hiện số lượng đối thủ vẫn còn trong trò chơi và số tiền tích lũy được. 
2. Ở mỗi bước, quyết định loại bỏ đúng một đối thủ, nghĩa là`t = 1`. Đây là phép loại bỏ khác 0 nhỏ nhất có thể và bất kỳ lô lớn hơn nào cũng sẽ không tối ưu vì nó sẽ sử dụng mẫu số lớn hơn trước đó. 
3. Thêm phần thưởng`1/s`để trả lời. Điều này phản ánh rằng có đúng một đối thủ bị loại khỏi`s`còn lại. 
4. Giảm`s`bởi một, vì đúng một đối thủ đã bị loại. 
5. Lặp lại cho đến khi`s`trở thành số không. Khi đó tất cả đối thủ đã bị loại và quá trình dừng lại. 

### Tại sao nó hoạt động 

Thuộc tính cốt lõi là việc phân chia bất kỳ sự loại bỏ kích thước nào`k`vào trong`k`các lần loại bỏ riêng biệt luôn làm tăng tổng phần thưởng vì mỗi phân số tiếp theo sử dụng mẫu số nhỏ hơn. Điều này tạo ra một cấu trúc bắt buộc: bất kỳ giải pháp tối ưu nào cũng có thể được chuyển thành một giải pháp trong đó mỗi vòng loại bỏ chính xác một đối thủ mà không làm giảm câu trả lời. Theo cấu trúc này, chuỗi các trạng thái là duy nhất`n, n-1, ..., 1`, do đó tổng phần thưởng là cố định và tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

ans = 0.0
for i in range(1, n + 1):
    ans += 1.0 / i

print(ans)
```Việc thực hiện trực tiếp tính toán tổng hài từ`1`ĐẾN`n`. Lặp đi lặp lại lên hoặc xuống là tương đương; ở đây chúng tôi tổng hợp`1/i`cho rõ ràng. 

Sự tinh tế quan trọng là sử dụng phép chia dấu phẩy động. Vì độ chính xác yêu cầu là`1e-4`, tích lũy độ chính xác kép tiêu chuẩn là đủ cho`n`lên tới`10^5`. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`n = 3`Chúng tôi tính tổng từng bước. 

| Bước | s | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 3 | 1/3 | 0,3333 | 
| 2 | 2 | 1/2 | 0,8333 | 
| 3 | 1 | 1/1 | 1.8333 | 

Câu trả lời cuối cùng là`1 + 1/2 + 1/3 = 1.8333...`. Điều này cho thấy mỗi bước tương ứng với việc loại bỏ chính xác một đối thủ như thế nào. 

### Ví dụ 2:`n = 5`| Bước | s | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 5 | 1/5 | 0,2 | 
| 2 | 4 | 1/4 | 0,45 | 
| 3 | 3 | 1/3 | 0,7833 | 
| 4 | 2 | 1/2 | 1.2833 | 
| 5 | 1 | 1/1 | 2.2833 | 

Điều này khẳng định cấu trúc tất định: mỗi trạng thái đóng góp chính xác theo tỷ lệ nghịch đảo của quy mô của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tính một tổng duy nhất cho tất cả các số nguyên từ 1 đến n | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Quét tuyến tính nhiều nhất`10^5`các giá trị dễ dàng phù hợp trong giới hạn thời gian và mức sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    n = int(sys.stdin.readline())
    ans = 0.0
    for i in range(1, n + 1):
        ans += 1.0 / i
    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided samples
assert abs(float(run("1")) - 1.0) < 1e-9

# custom cases
assert abs(float(run("2")) - (1 + 1/2)) < 1e-9
assert abs(float(run("3")) - (1 + 1/2 + 1/3)) < 1e-9
assert abs(float(run("10")) - sum(1/i for i in range(1, 11))) < 1e-9
assert abs(float(run("100000")) > 0)  # sanity check
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp tối thiểu | 
| 2 | 1,5 | hành vi điều hòa cơ bản | 
| 3 | 1.8333… | tích lũy nhiều bước | 
| 100000 | tổng điều hòa lớn | hiệu suất và sự ổn định | 

## Vỏ cạnh 

Khi nào`n = 1`, thuật toán thực hiện đúng một phép cộng`1/1`. Bất kỳ cách tiếp cận nào giả định có ít nhất một “sự lựa chọn” giữa các quy mô loại bỏ khác nhau sẽ thất bại ở đây vì không có sự phân nhánh nào cả. 

Khi`n`lớn, chẳng hạn như`100000`, tổng tăng chậm và phụ thuộc vào nhiều phần đóng góp nhỏ. Các phương pháp cố gắng mô phỏng trình tự loại bỏ một cách rõ ràng sẽ vượt quá giới hạn thời gian, trong khi tổng hài hòa vẫn tuyến tính và ổn định. 

Khi xem xét độ chính xác của dấu phẩy động, thứ tự tích lũy không ảnh hưởng đáng kể đến độ chính xác ở dung sai yêu cầu, nhưng việc trộn số nguyên hoặc truyền không chính xác sẽ ngay lập tức phá vỡ kết quả.
