---
title: "CF 102307I - Tiền tố số nguyên"
description: "Chúng tôi được cung cấp một chuỗi văn bản không có khoảng trắng. Nhiệm vụ là tìm tiền tố dài nhất của chuỗi đó có các ký tự đều là chữ số thập phân. Tiền tố phải không trống. Nếu ký tự đầu tiên không phải là chữ số thì không tồn tại tiền tố hợp lệ và chúng ta in -1."
date: "2026-08-13T07:23:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "I"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 65
verified: true
draft: false
---

[CF 102307I - Tiền tố số nguyên](https://codeforces.com/problemset/problem/102307/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi văn bản không có khoảng trắng. Nhiệm vụ là tìm tiền tố dài nhất của chuỗi đó có các ký tự đều là chữ số thập phân. Tiền tố phải không trống. Nếu ký tự đầu tiên không phải là chữ số thì không tồn tại tiền tố hợp lệ và chúng ta in`-1`. 

Ví dụ, trong`23082019UNAL`, các ký tự đầu tiên`23082019`là các chữ số, trong khi ký tự tiếp theo`U`thì không. Câu trả lời là do đó`23082019`. TRONG`ABC123`, ký tự đầu tiên đã phá vỡ điều kiện chỉ có chữ số, vì vậy câu trả lời là`-1`. 

Chuỗi chứa nhiều nhất`2 * 10^5`nhân vật. Kích thước đó đủ lớn để thuật toán bậc hai không phù hợp, đặc biệt là trong giới hạn thời gian một giây. Một giải pháp thực hiện đại khái`n^2`kiểm tra ký tự có thể đạt tới khoảng`4 * 10^10`hoạt động ở độ dài tối đa, vượt xa những gì có thể phù hợp với giới hạn. Về cơ bản, chúng ta cần kiểm tra chuỗi một lần, đưa ra`O(n)`giải pháp. 

Có một vài trường hợp ranh giới có thể khiến việc triển khai đơn giản không thành công. Nếu đầu vào là`7`, toàn bộ chuỗi là tiền tố số hợp lệ, vì vậy đầu ra là`7`. Việc triển khai mong đợi một dấu kết thúc không có chữ số sẽ bỏ lỡ trường hợp này một cách không chính xác. 

Nếu đầu vào là`A123`, đầu ra đúng là`-1`, vì tiền tố hợp lệ phải bắt đầu ở ký tự đầu tiên. Một giải pháp bất cẩn khi tìm kiếm bất kỳ dãy chữ số nào có thể trả về sai`123`. 

Nếu đầu vào là`123A456`, câu trả lời là`123`, không`123456`. Khi chữ số đầu tiên không xuất hiện, không có ký tự nào sau này có thể thuộc về tiền tố bắt buộc. 

Nếu đầu vào là`0000X`, câu trả lời là`0000`. Tiền tố là một chuỗi chứ không phải số nguyên, vì vậy các số 0 đứng đầu phải được giữ nguyên. Chuyển đổi tiền tố thành số nguyên trước khi in sẽ tạo ra sai số`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể xem xét mọi tiền tố có thể có và kiểm tra xem mọi ký tự bên trong tiền tố đó có phải là chữ số hay không. Đối với một chuỗi có độ dài`n`, có`n`tiền tố có thể không trống. Nếu chúng ta kiểm tra tiền tố kết thúc ở vị trí`i`ngay từ đầu mỗi lần, tổng số lần kiểm tra ký tự là`1 + 2 + 3 + ... + n = n(n + 1)/2`. 

Tại`n = 200000`, đây là về`2 * 10^10`kiểm tra. Phương pháp brute-force đúng về mặt logic vì nó xác minh rõ ràng định nghĩa của tiền tố hợp lệ, nhưng nó liên tục kiểm tra các ký tự đã được biết là chữ số. 

Cấu trúc của bài toán cho chúng ta một quan sát đơn giản hơn nhiều. Tiền tố có giá trị chính xác trong khi mọi ký tự gặp từ đầu đều là một chữ số. Ký tự không có chữ số đầu tiên sẽ kết thúc vĩnh viễn câu trả lời. Không có lý do gì để xem xét lại ký tự trước đó sau khi tiếp cận nó, vì tiền tố không thể bỏ qua ký tự. 

Điều đó cho phép chúng tôi quét từ trái sang phải một lần. Tại mỗi vị trí nếu ký tự là chữ số thì ta tiếp tục. Ở chữ số đầu tiên, chúng tôi dừng và xuất mọi thứ trước vị trí đó. Nếu quá trình quét đến cuối, toàn bộ chuỗi là câu trả lời. Điều này làm giảm công việc từ bậc hai sang tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào hoàn chỉnh và xóa dòng mới ở cuối. Chúng tôi giữ nó dưới dạng một chuỗi thay vì chuyển đổi nó thành số, vì các số 0 đứng đầu là một phần của kết quả đầu ra được yêu cầu. 
2. Bắt đầu quét từ ký tự đầu tiên đến cuối chuỗi. Quá trình quét thể hiện tiền tố dài nhất đã được xác nhận là chỉ chứa các chữ số cho đến nay. 
3. Khi ký tự hiện tại là một chữ số, hãy tiếp tục quét. Mọi ký tự trước vị trí hiện tại đều đã được xác minh, do đó việc mở rộng tiền tố thêm một chữ số khác vẫn hợp lệ. 
4. Khi ký tự hiện tại không phải là chữ số, dừng ngay và in chuỗi con trước vị trí này. Đây là câu trả lời dài nhất có thể vì mỗi tiền tố dài hơn sẽ chứa cùng một ký tự không phải chữ số. 
5. Nếu quá trình quét đến cuối mà không tìm thấy chữ số nào, hãy in toàn bộ chuỗi. Mọi ký tự đều đã được xác minh dưới dạng chữ số, vì vậy chuỗi hoàn chỉnh là tiền tố hợp lệ dài nhất. 
6. Nếu ký tự đầu tiên không phải là số thì vị trí dừng bằng 0 và tiền tố trước nó trống. Vì vấn đề yêu cầu tiền tố không trống, hãy in`-1`thay vì. 

### Tại sao nó hoạt động 

Giữ nguyên bất biến trước vị trí`i`, mọi ký tự trong tiền tố`T[0:i]`là một chữ số. Nếu như`T[i]`cũng là một chữ số, việc mở rộng tiền tố sẽ giữ nguyên bất biến. Nếu như`T[i]`không phải là một chữ số, mỗi tiền tố dài hơn`T[0:i]`chứa`T[i]`, vì vậy không có tiền tố nào trong số đó có thể hợp lệ. Do đó, vị trí không có chữ số đầu tiên xác định duy nhất tiền tố số dài nhất. Nếu không có vị trí đó thì mỗi ký tự là một chữ số và toàn bộ chuỗi là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    for i, ch in enumerate(s):
        if not ch.isdigit():
            if i == 0:
                print(-1)
            else:
                print(s[:i])
            return

    print(s)

if __name__ == "__main__":
    solve()
```Dòng đầu vào được đọc một lần và`strip()`loại bỏ dòng mới được thêm vào bởi đầu vào tiêu chuẩn. Bản thân chuỗi đó không bao giờ được chuyển đổi thành số nguyên, số này giữ nguyên các số 0 đứng đầu chẳng hạn như các số trong`000123`. 

Vòng lặp kiểm tra các ký tự theo thứ tự ban đầu của chúng. Càng sớm càng`isdigit()`trả về sai, vòng lặp đã tìm thấy ranh giới chính xác giữa tiền tố số và phần còn lại của văn bản. lát cắt`s[:i]`chứa chính xác các ký tự trước ranh giới đó. 

các`i == 0`check xử lý yêu cầu không trống. Nếu ký tự đầu tiên không phải là chữ số,`s[:0]`sẽ là một chuỗi rỗng, vì vậy kết quả đầu ra được yêu cầu là`-1`thay vì. 

Không có số học số nguyên trong giải pháp, vì vậy việc tràn số nguyên là không liên quan. Ranh giới duy nhất quan trọng là liệu ký tự không hợp lệ đầu tiên có xuất hiện ở chỉ số 0, ở đâu đó ở giữa hay không. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`23082019UNAL`, quá trình quét tiếp tục qua tất cả tám chữ số ban đầu và dừng ở`U`. 

| Chỉ mục | Nhân vật | Là chữ số? | Tiền tố được xác nhận | 
| --- | --- | --- | --- | 
| 0 |`2`| Có |`2`| 
| 1 |`3`| Có |`23`| 
| 2 |`0`| Có |`230`| 
| 3 |`8`| Có |`2308`| 
| 4 |`2`| Có |`23082`| 
| 5 |`0`| Có |`230820`| 
| 6 |`1`| Có |`2308201`| 
| 7 |`9`| Có |`23082019`| 
| 8 |`U`| Không | Dừng lại | 

Chữ số không đầu tiên nằm ở chỉ mục`8`, vậy câu trả lời là`s[:8]`, đó là`23082019`. Các ký tự còn lại không quan trọng vì mỗi tiền tố dài hơn sẽ chứa`U`. 

### Mẫu 2 

cho`UNAL`, ký tự đầu tiên không phải là chữ số. 

| Chỉ mục | Nhân vật | Là chữ số? | Hành động | 
| --- | --- | --- | --- | 
| 0 |`U`| Không | Dừng lại và in`-1`| 

Không có tiền tố nào trống chỉ bao gồm các chữ số. Dấu vết này thực hiện ranh giới nơi ký tự không hợp lệ đầu tiên xuất hiện ở vị trí 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra nhiều nhất một lần. | 
| Không gian | O(n) | Chuỗi đầu vào yêu cầu bộ nhớ O(n) và lát cắt đầu ra cũng có thể yêu cầu bộ nhớ O(n). | 

Với nhiều nhất`2 * 10^5`ký tự, một lần quét tuyến tính chỉ thực hiện kiểm tra vài trăm nghìn ký tự. Điều này thoải mái trong giới hạn thời gian một giây đã nêu, trong khi phương pháp tiếp cận vũ phu bậc hai có thể yêu cầu khoảng`2 * 10^10`séc. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_string(s: str) -> str:
    for i, ch in enumerate(s):
        if not ch.isdigit():
            return "-1" if i == 0 else s[:i]
    return s

def run(inp: str) -> str:
    return solve_string(inp.strip())

# Provided samples
assert run("23082019UNAL") == "23082019", "sample 1"
assert run("UNAL") == "-1", "sample 2"

# Minimum-size inputs
assert run("7") == "7", "single digit"
assert run("A") == "-1", "single non-digit"

# Leading zeroes must be preserved
assert run("0000X") == "0000", "leading zeroes"

# Non-digit immediately after the numeric prefix
assert run("123A456") == "123", "prefix boundary"

# All characters are digits
assert run("999999") == "999999", "all digits"

# Maximum-size input, with the first non-digit at the very end
maximum = "7" * (2 * 10**5 - 1) + "X"
assert run(maximum) == "7" * (2 * 10**5 - 1), "maximum-size input"

# Maximum-size input with no valid prefix
maximum_invalid = "X" + "7" * (2 * 10**5 - 1)
assert run(maximum_invalid) == "-1", "maximum-size invalid prefix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`|`7`| Kích thước tối thiểu và tiền tố ở cuối | 
|`A`|`-1`| Kích thước tối thiểu không có tiền tố số | 
|`0000X`|`0000`| Bảo toàn các số 0 đứng đầu | 
|`123A456`|`123`| Ranh giới dừng đúng | 
|`999999`|`999999`| Toàn bộ chuỗi là số | 
|`777...7X`với chiều dài`200000`| Tất cả trừ cuối cùng`X`| Kích thước đầu vào tối đa và ranh giới ký tự cuối cùng | 
|`X777...7`với chiều dài`200000`|`-1`| Kích thước đầu vào tối đa không có tiền tố hợp lệ | 

## Vỏ cạnh 

cho`A123`, thuật toán kiểm tra chỉ số`0`, thấy thế`A`không phải là một chữ số và ngay lập tức trả về`-1`. Nó không bao giờ tìm kiếm phân đoạn số sau, vì đối tượng được yêu cầu là tiền tố bắt đầu ở ký tự đầu tiên. 

Vì`123A456`, chỉ số`0`,`1`, Và`2`được chấp nhận. Tại chỉ mục`3`, nhân vật`A`thất bại trong việc kiểm tra chữ số, do đó thuật toán trả về`s[:3]`, đó là`123`. Hậu tố`456`bị cố tình bỏ qua vì tiền tố không thể bỏ qua`A`. 

Vì`0000X`, mọi số 0 đều vượt qua bài kiểm tra chữ số và quá trình quét chỉ dừng ở`X`. Chuỗi con trả về là`0000`, bảo toàn cả bốn số 0. Coi câu trả lời là số nguyên sẽ làm mất thông tin mà bài toán yêu cầu chúng ta in ra. 

Vì`7`, vòng lặp không bao giờ gặp một chữ số nào. Nó đến cuối và trả về chuỗi hoàn chỉnh`7`. Điều này xử lý trường hợp không có ký tự không có chữ số kết thúc. 

Đối với một chuỗi có độ dài`200000`bao gồm toàn bộ các chữ số, mỗi ký tự được xử lý một lần và trả về chuỗi hoàn chỉnh. Thuật toán không cần nhánh có độ dài tối đa đặc biệt, do đó ràng buộc trên được xử lý một cách tự nhiên. 

Đối với một chuỗi có ký tự đầu tiên không phải là số và các ký tự còn lại`199999`ký tự là chữ số, thuật toán vẫn dừng sau một ký tự và trả về`-1`. Điều này chứng tỏ tại sao câu trả lời chỉ phụ thuộc vào chuỗi chữ số đầu tiên không bị gián đoạn mà không phụ thuộc vào sự hiện diện của các chữ số ở phần sau của văn bản.
