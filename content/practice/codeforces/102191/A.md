---
title: "CF 102191A - Người ăn uống hào phóng"
description: "Chúng ta bắt đầu với n chiếc kẹo và muốn tặng kẹo cho càng nhiều người bạn khác nhau càng tốt, mỗi người một chiếc kẹo. Sau khi mỗi người bạn thứ hai nhận được một viên kẹo, chúng ta sẽ tự ăn một viên kẹo nếu còn sót lại viên kẹo nào."
date: "2026-08-23T09:12:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "A"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 1443
verified: true
draft: false
---

[CF 102191A - Kẻ ăn uống hào phóng](https://codeforces.com/problemset/problem/102191/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với`n`kẹo và muốn tặng kẹo cho càng nhiều bạn bè khác nhau càng tốt, mỗi người bạn một viên kẹo. Sau khi mỗi người bạn thứ hai nhận được một viên kẹo, chúng ta sẽ tự ăn một viên kẹo nếu còn sót lại viên kẹo nào. Quá trình tiếp tục cho đến khi chúng ta không thể tặng thêm một viên kẹo nào cho bạn bè nữa. 

Nhiệm vụ là tính số lượng bạn bè tối đa có thể nhận được kẹo. 

Giá trị của`n`có thể lớn như`10^9`. Điều đó ngay lập tức loại trừ bất kỳ mô phỏng nào thực hiện một thao tác trên mỗi viên kẹo, vì trong trường hợp xấu nhất, nó sẽ thực hiện khoảng một tỷ lần lặp. Ngay cả với lượng công việc không đổi nhỏ trên mỗi lần lặp, điều đó vẫn vượt xa những gì mà một giải pháp lập trình cạnh tranh dưới giây có thể thực hiện được. Chúng ta cần rút ra câu trả lời trực tiếp từ cấu trúc của quy trình. 

Những trường hợp tế nhị nhất xảy ra vào thời điểm không còn kẹo để ăn. Ví dụ, với đầu vào`2`, câu trả lời là`2`, không`1`. Chúng tôi đưa cho mỗi người một chiếc kẹo, sau đó không còn gì để ăn nên cả hai người bạn đều được phục vụ. Một công thức bất cẩn luôn trừ đi một viên kẹo sau mỗi cặp sẽ từ chối người bạn thứ hai một cách không chính xác. 

Một trường hợp ranh giới khác là`4`. Câu trả lời đúng là`3`. Chúng ta tặng kẹo cho hai người bạn đầu tiên, ăn một viên kẹo và còn lại một viên kẹo cho người bạn thứ ba. Sau đó, quá trình dừng lại. Một phép tính ngây thơ về việc cứ hai người bạn ăn một chiếc kẹo có thể mong đợi một cách sai lầm là sẽ có nhiều hơn bốn chiếc kẹo để phục vụ ba người bạn. 

đầu vào`6`là một trường hợp ranh giới hữu ích khác. Câu trả lời là`4`, không`5`. Sau khi phục vụ hai người bạn, một chiếc kẹo được ăn, còn lại ba chiếc. Sau đó chúng tôi phục vụ thêm hai người bạn nữa, để lại một người và chiếc kẹo cuối cùng được ăn. Chẳng còn gì cho người bạn thứ năm. Điều này nắm bắt các công thức coi mọi viên kẹo còn lại đều tự động có sẵn cho bạn bè. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể mô phỏng quy trình thực tế. Giữ số lượng kẹo, liên tục tặng một viên kẹo cho một người bạn mới và sau mỗi người bạn thứ hai ăn một viên kẹo bất cứ khi nào có thể. Mô phỏng là đúng vì nó tuân theo chính xác các quy tắc của quy trình. Tuy nhiên, nó có thể thực hiện Θ(`n`) lần lặp. Với`n = 10^9`, điều đó có nghĩa là lên tới một tỷ lần lặp, quá chậm so với giới hạn thời gian. 

Quan sát hữu ích là quá trình này có một mẫu lặp lại đơn giản. Hãy xem xét ba viên kẹo. Họ có thể tạo ra hai người bạn: hai chiếc kẹo được chia cho hai người bạn và chiếc kẹo thứ ba được ăn. Như vậy, mỗi nhóm ba viên kẹo hoàn chỉnh sẽ đóng góp hiệu quả cho hai người bạn. Những viên kẹo còn lại sau những nhóm hoàn chỉnh đó đều có thể được tặng cho những người bạn khác vì có ít hơn ba người trong số họ và họ không thể ép thêm một chu kỳ ăn đầy đủ nữa. 

Cho phép`n = 3q + r`, Ở đâu`r`là`0`,`1`, hoặc`2`. 

các`q`hoàn thành nhóm ba viên kẹo có nguyên nhân chính xác`q`kẹo để ăn, trong khi phần còn lại`r`kẹo đi cho bạn bè. Vậy số bạn bè là`n - q = n - floor(n / 3)`. 

Kết quả tương tự có thể được hiểu theo hướng ngược lại. Để phục vụ`k`các bạn, quá trình này cần`k`kẹo được tặng cho bạn bè cộng với một viên kẹo đã ăn sau mỗi cặp được một người bạn khác theo sau. Điều này tạo ra cùng một tỷ lệ giữa hai viên kẹo hữu ích cho mỗi ba viên kẹo được tiêu thụ, trong đó nhóm chưa hoàn thành cuối cùng được xử lý trực tiếp bởi những viên còn lại. 

Vì vậy, toàn bộ mô phỏng thu gọn thành một phép chia số nguyên và một phép trừ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số kẹo`n`. Thông tin duy nhất cần thiết là tổng số lượng, vì quá trình này không phân biệt giữa kẹo hay bạn bè. 

2. Tính toán`n // 3`. Đây là số nhóm hoàn chỉnh gồm ba viên kẹo. Mỗi nhóm như vậy có một viên kẹo phải được ăn thay vì đưa cho bạn bè. 

3. Trừ số đó khỏi`n`. Kết quả,`n - n // 3`, là số kẹo cuối cùng có thể tặng cho bạn bè. 

4. In kết quả. 

Tại sao việc nhóm theo ba viên kẹo lại có tác dụng ngay cả khi nhóm cuối cùng có một hoặc hai viên kẹo? Một nhóm hoàn chỉnh có một viên kẹo đã ăn và hai viên kẹo được tặng cho bạn bè. Phần còn lại của một hoặc hai viên kẹo sẽ xuất hiện sau tất cả các chu kỳ ăn hoàn chỉnh và có thể được tặng trực tiếp cho bạn bè, do đó không cần trừ thêm. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi khối hoàn chỉnh gồm ba viên kẹo được tiêu thụ sẽ làm giảm số lượng kẹo có sẵn cho bạn bè đi đúng một. Hai viên kẹo trong một khối như vậy được tặng cho bạn bè, còn một viên sẽ được ăn. Nếu như`n = 3q + r`, có chính xác`q`khối hoàn chỉnh và`r < 3`kẹo còn sót lại. Các khối hoàn chỉnh chiếm chính xác`q`ăn kẹo, trong khi tất cả`r`thức ăn thừa có thể được tặng cho bạn bè. Như vậy chính xác`q = floor(n / 3)`ăn hết kẹo và số lượng bạn bè tối đa là`n - floor(n / 3)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
print(n - n // 3)
```Đầu vào là một số nguyên nên chương trình sẽ đọc trực tiếp dưới dạng`n`. Số nguyên Python dễ dàng xử lý`10^9`, mặc dù ở đây ngay cả số nguyên có dấu 32 bit cũng đủ. 

biểu thức`n // 3`thực hiện phép chia số nguyên và đưa ra số nhóm hoàn chỉnh gồm ba. Trừ nó từ`n`đưa ra số lượng kẹo đến tay bạn bè. 

Không có vòng lặp nên không có ranh giới mô phỏng hoặc chuyển tiếp từng cái một để quản lý. Đặc biệt, sử dụng phép chia số nguyên là cách xử lý chính xác các giá trị như`2`,`4`, Và`5`: thương của chúng với 3 là`0`,`1`, Và`1`, tương ứng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`n = 4`, phép chia số nguyên cho`4 // 3 = 1`. Ăn hết một viên kẹo, để lại ba viên kẹo có thể tặng bạn bè. 

|`n`|`n // 3`| Bạn`n - n // 3`| 
|---:|---:|---:| 
| 4 | 1 | 3 | 

Quá trình tương ứng là tặng kẹo cho hai người bạn, ăn một viên kẹo, sau đó đưa viên kẹo còn lại cho người bạn thứ ba. Đầu ra là`3`. 

### Mẫu 2 

cho`n = 5`, vẫn chỉ có một nhóm đầy đủ ba người, vậy nên ăn mất đúng một viên kẹo. Bốn viên kẹo còn lại đến tay bạn bè. 

|`n`|`n // 3`| Bạn`n - n // 3`| 
|---:|---:|---:| 
| 5 | 1 | 4 | 

Quá trình này có thể phục vụ bốn người bạn: hai người nhận kẹo, một viên kẹo được ăn và hai viên kẹo còn lại dành cho hai người bạn nữa. Đầu ra là`4`. 

Những ví dụ này cũng cho thấy tại sao phần còn lại lại quan trọng. Sau khi loại bỏ một nhóm ba viên hoàn chỉnh trong số năm viên kẹo, vẫn còn hai viên kẹo và cả hai đều có thể được cho đi mà không cần tạo ra một chu kỳ ăn khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(1) | Giải pháp thực hiện một phép chia số nguyên và một phép trừ. | 
| Không gian | O(1) | Chỉ sử dụng số nguyên đầu vào và lượng lưu trữ tạm thời không đổi. | 

Đầu vào lớn nhất có thể là`10^9`, nhưng thuật toán không bao giờ lặp lại các viên kẹo. Nó thực hiện một số phép tính số học cố định, do đó nó phù hợp thoải mái với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    print(n - n // 3)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    output = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output

# provided samples
assert run("4\n") == "3\n", "sample 1"
assert run("5\n") == "4\n", "sample 2"
assert run("6\n") == "4\n", "sample 3"

# custom cases
assert run("1\n") == "1\n", "minimum input"
assert run("2\n") == "2\n", "no candy remains to eat after the second friend"
assert run("7\n") == "5\n", "two complete groups plus one remainder"
assert run("1000000000\n") == "666666667\n", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---:|---| 
|`1`|`1`| Đầu vào có kích thước tối thiểu và không có cặp bạn bè | 
|`2`|`2`| Ranh giới nơi người bạn thứ hai có thể được phục vụ mà không cần ăn thêm kẹo | 
|`7`|`5`| Phần còn lại sau hai nhóm ba hoàn chỉnh | 
|`1000000000`|`666666667`| Đầu vào tối đa và số học theo thời gian không đổi | 

## Vỏ cạnh 

cho`n = 1`, thuật toán tính toán`1 // 3 = 0`, vậy câu trả lời là`1`. Chỉ có một viên kẹo và nó sẽ được chuyển trực tiếp đến một người bạn. Không có chu kỳ ăn uống được kích hoạt. 

Vì`n = 2`, thuật toán tính toán`2 // 3 = 0`, đưa ra câu trả lời`2`. Chúng ta có thể phục vụ cả hai người bạn, và sau chiếc kẹo thứ hai thì không còn viên kẹo nào để ăn. Đây là ranh giới phá vỡ việc triển khai loại bỏ một cách mù quáng một viên kẹo sau mỗi cặp. 

Vì`n = 4`, thuật toán tính toán`4 // 3 = 1`, cho`4 - 1 = 3`. Trình tự thực tế là hai viên kẹo được tặng, một viên được ăn và viên kẹo cuối cùng được trao cho người bạn thứ ba. Nhóm ba người hoàn chỉnh duy nhất chiếm một viên kẹo được ăn. 

Vì`n = 6`, thuật toán tính toán`6 // 3 = 2`, cho`6 - 2 = 4`. Hai người bạn đầu tiên ăn hai viên kẹo và kích hoạt một viên kẹo đã ăn. Hai người bạn tiếp theo ăn thêm hai viên kẹo nữa và kích hoạt viên kẹo ăn thứ hai. Số kẹo còn lại không thể hỗ trợ người bạn thứ năm, vì vậy bốn viên là tối đa. 

Vì`n = 10^9`, thuật toán thực hiện chính xác lượng công việc không đổi như đối với bất kỳ đầu vào nhỏ hơn nào. Từ`10^9 // 3 = 333333333`, câu trả lời là`1000000000 - 333333333 = 666666667`. Điều này khẳng định rằng nghiệm không phụ thuộc vào độ lớn của`n`thông qua số lần lặp.
