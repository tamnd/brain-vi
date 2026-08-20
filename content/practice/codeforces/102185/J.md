---
title: "CF 102185J - \u041a\u043e\u0440\u043c\u043b\u0435\u043d\u0438\u0435 \u043a\u0440\u043e\u043a\u043e\u0434\u0438\u043b\u043e\u0432"
description: "Chúng ta có một lượng thịt chẵn N và hai con cá sấu. Vasya chọn hai miếng nguyên dương cỡ A và B. Tổng số miếng thịt phải bao gồm số miếng mỗi cỡ bằng nhau, vì vậy nếu có m miếng cỡ A và m miếng cỡ B thì N = m(A + B)."
date: "2026-08-19T06:38:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "J"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 86
verified: true
draft: false
---

[CF 102185J - \u041a\u043e\u0440\u043c\u043b\u0435\u043d\u0438\u0435 \u043a\u0440\u043e\u043a\u043e\u0434\u0438\u043b\u043e\u0432](https://codeforces.com/problemset/problem/102185/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lượng thịt đồng đều,`N`, và hai con cá sấu. Vasya chọn hai kích thước mảnh nguyên dương`A`Và`B`. Tổng số thịt phải bao gồm cùng số miếng ở mỗi kích cỡ, vì vậy nếu có`m`miếng có kích thước`A`Và`m`miếng có kích thước`B`, sau đó`N = m(A + B)`. 

Các mảnh được ném mỗi`K`giây theo thứ tự`A, B, A, B, ...`. Cá sấu chỉ có thể bắt được miếng mới khi nó đã ăn xong miếng trước đó. Nếu cả hai con cá sấu đều được tự do cùng một lúc thì cá sấu mạnh hơn sẽ giành được phần đó. Mỗi kg mất đúng một giây để ăn. 

Chúng ta cần xuất ra bất kỳ số nguyên dương nào`A`Và`B`trong đó sự khác biệt giữa tổng số lượng thức ăn mà hai con cá sấu ăn là càng nhỏ càng tốt. 

Sự ràng buộc`N <= 10^9`ngay lập tức loại trừ việc liệt kê tất cả các cặp kích cỡ mảnh có thể có. Có thể có theo thứ tự`N^2`các cặp dương, vượt xa những gì giải pháp một giây có thể kiểm tra. Điều quan trọng là câu trả lời chỉ phụ thuộc vào cách`K`so sánh với`N/2`Và`N-1`, do đó thuật toán cuối cùng chỉ cần thời gian không đổi và bộ nhớ không đổi. 

Có ba tình huống ranh giới mà một giải pháp bất cẩn có thể xử lý sai. Vì`N = 4, K = 1`, đang chọn`A = B = 2`là tối ưu. Miếng đầu tiên được con cá sấu khỏe bắt được ở thời điểm 0, nhưng con cá sấu đó vẫn đang ăn khi miếng thứ hai đến vào thời điểm một nên con cá sấu yếu đã ăn miếng đó. Một mô phỏng kiểm tra xem cá sấu có hoàn thành đúng trước lần ném tiếp theo hay không thay vì cho phép tính kết thúc đồng thời nếu có thể, trường hợp này có thể sai. 

Vì`N = 4, K = 2`, đang chọn`A = B = 2`không cân bằng được thức ăn. Con cá sấu mạnh mẽ về đích đúng lúc quân thứ hai được ném ra nên ngay lập tức đủ điều kiện và bắt luôn quân đó. Cách xây dựng tối ưu là`A = 3, B = 1`, cho cá sấu mạnh 3 kg và cá sấu yếu 1 kg. Sự bình đẳng ở ranh giới là nguồn gốc của lỗi chung. 

Vì`N = 4, K = 3`, không quân nào có thể khiến con cá sấu mạnh mẽ bận rộn cho đến lần ném thứ hai, bởi vì ngay cả quân đầu tiên lớn nhất có thể cũng có kích thước tối đa là 3. Công trình xây dựng`A = 1, B = 3`hợp lệ nhưng con cá sấu khỏe mạnh bắt được cả hai miếng và ăn hết 4 kg. Một giải pháp cho rằng con cá sấu yếu phải nhận được một ít thịt sẽ tìm kiếm một cách sai lầm sự cân bằng không thể có được. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể liệt kê mọi cặp dương`(A, B)`, bác bỏ các cặp mà`A + B`không chia`N`và mô phỏng tất cả các lần ném cho mọi cặp còn lại. Mô phỏng này là chính xác vì trạng thái của mỗi con cá sấu hoàn toàn được xác định bởi thời điểm nó bắt được một miếng cuối cùng và kích thước của miếng đó. 

Vấn đề là số lượng ứng viên. Nếu chúng ta chỉ đơn giản liệt kê tất cả những điều tích cực`A`Và`B`với`A + B <= N`, có`1 + 2 + ... + (N - 1) = N(N - 1)/2`các cặp có thứ tự, nghĩa là khoảng`5 * 10^17`khi`N = 10^9`. Ngay cả việc loại bỏ các cặp không hợp lệ trước khi mô phỏng cũng không thể làm cho phương pháp đó khả thi. 

Cấu trúc trở nên đơn giản hơn nhiều khi chúng ta nhìn vào số`m`của các cặp mảnh. Từ`N = m(A+B)`, nếu như`m >= 2`, sau đó`A + B <= N/2`. 

Sự bất bình đẳng duy nhất này có tính chất quyết định. Khi`K >= N/2`, mỗi mảnh trong công trình như vậy có kích thước tối đa`K`, vì vậy sau khi bắt được một miếng, con cá sấu mạnh mẽ sẽ được tự do ở lần ném tiếp theo. Vì con cá sấu mạnh mẽ sẽ chiến thắng bất cứ khi nào nó được tự do nên nó sẽ bắt được mọi mảnh ghép. 

Như vậy, khi`K >= N/2`, bất kỳ nỗ lực hữu ích nào để cung cấp thịt cho con cá sấu yếu đuối đều phải sử dụng`m = 1`. Khi đó có đúng hai mảnh có kích thước`A`Và`B`, với`A+B=N`. Miếng đầu tiên thuộc về con cá sấu mạnh mẽ. Miếng thứ hai chỉ có thể đến tay con cá sấu yếu đuối nếu miếng đầu tiên vẫn đang được ăn`K`, có nghĩa là`A > K`. 

Điều này làm giảm toàn bộ việc tối ưu hóa thành việc chọn kích thước nhỏ nhất có thể`A`thỏa mãn`A > K`. Đó là`A = K+1`, cho`B = N-K-1`, bất cứ khi nào`K <= N-2`. 

Khi`K < N/2`, thậm chí còn có một khả năng tốt hơn. Chúng ta có thể lấy một mảnh của mỗi kích thước với`A = B = N/2`. Từ`A > K`, con cá sấu mạnh mẽ vẫn đang bận rộn khi miếng thứ hai đến. Cá sấu yếu bắt được nên cả hai cá sấu đều nhận được chính xác`N/2`kg và chênh lệch tối ưu là bằng không. 

Khi`K >= N-1`, ngay cả mảnh đầu tiên lớn nhất có thể có trong kết cấu hai mảnh cũng không thể đáp ứng được`A > K`, bởi vì`A <= N-1`. Con cá sấu yếu đuối không thể nhận được gì nên sự khác biệt khó tránh khỏi là`N`. Chúng ta có thể sử dụng`A = 1, B = N-1`, khiến con cá sấu mạnh mẽ ăn hết mọi thứ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. So sánh`K`với`N/2`. Nếu như`K < N/2`, chọn`A = B = N/2`. Mảnh đầu tiên khiến con cá sấu khỏe mạnh bận rộn hơn`K`giây, vậy là miếng thứ hai thuộc về con cá sấu yếu đuối. Cả hai đều nhận được số tiền như nhau, đó là mức chênh lệch tối thiểu tuyệt đối có thể có, bằng không. 
2. Nếu`K >= N/2`, hãy xem xét liệu`K < N-1`. Trong phạm vi này, chọn`A = K+1`Và`B = N-K-1`. Phần đầu tiên kéo dài hơn`K`giây, vì vậy con cá sấu mạnh mẽ đang bận rộn khi miếng thứ hai đến. Con cá sấu yếu đuối nhận được miếng thứ hai. Từ`A`là mảnh đầu tiên nhỏ nhất có thể tồn tại sau lần ném tiếp theo, điều này giảm thiểu sự khác biệt thu được`A-B`. 
3. Nếu`K >= N-1`, chọn`A = 1`Và`B = N-1`. Con cá sấu khỏe mạnh hoàn thành miếng đầu tiên trước khi ném miếng thứ hai và cũng bắt được miếng thứ hai. Nó ăn hết`N`kg, và không có công trình xây dựng nào có thể cho con cá sấu yếu đuối chút thịt nào, vì vậy đây là điều tối ưu. 

### Tại sao nó hoạt động 

Bất biến trung tâm là bất cứ khi nào`A+B <= N/2`Và`K >= N/2`, cả hai kích thước mảnh có thể có nhiều nhất`K`, nên con cá sấu mạnh mẽ luôn được tự do ở lần ném tiếp theo. Do đó, mọi công trình có ít nhất hai bản sao ở mỗi kích cỡ đều cung cấp mọi thứ cho con cá sấu mạnh mẽ. 

Khi`K < N/2`, việc xây dựng`A=B=N/2`mang lại sự khác biệt bằng 0, không thể cải thiện được. Khi`K >= N/2`, bất kỳ công trình nào có thể nuôi cá sấu yếu phải có chính xác một miếng cho mỗi kích cỡ, bởi vì tất cả các công trình có ít nhất hai cặp đều nuôi mọi thứ cho cá sấu mạnh. Với hai miếng, con cá sấu yếu nhận được miếng thứ hai đúng lúc`A>K`, do đó giá trị nhỏ nhất có thể có`A`là`K+1`. Điều này giảm thiểu`A-B`, chứng tỏ rằng mỗi nhánh trả về một kết cấu tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())

    if 2 * K < N:
        A = N // 2
        B = N // 2
    elif K < N - 1:
        A = K + 1
        B = N - K - 1
    else:
        A = 1
        B = N - 1

    print(A, B)

solve()
```Nhánh đầu tiên sử dụng`2 * K < N`thay vì`K < N / 2`, tránh số học dấu phẩy động. Từ`N`là chẵn,`N/2`là một số nguyên, và bất đẳng thức chặt chẽ chính xác là điều kiện mà một phần có kích thước`N/2`vẫn đang được ăn khi miếng tiếp theo đến. 

Ở nhánh thứ hai,`A = K+1`cố tình là một lớn hơn`K`. Nếu chúng ta sử dụng`A = K`, con cá sấu khỏe mạnh sẽ hoàn thành miếng đầu tiên đúng ở lần ném thứ hai và đủ điều kiện để bắt được miếng đó. Vấn đề nói rõ ràng rằng việc hoàn thiện và ném đồng thời cho phép cá sấu thử miếng mới. 

biểu hiện`B = N-K-1`là tích cực chính xác trong khi`K < N-1`. Đó là lý do tại sao nhánh thứ hai dừng lại ở`K = N-1`. Ở nhánh cuối cùng,`A = 1`Và`B = N-1`đều dương và có tổng bằng`N`, vậy có chính xác một mảnh cho mỗi kích cỡ. 

Tất cả các giá trị nhiều nhất`10^9`, do đó số nguyên Python dễ dàng xử lý mọi phép tính. Không cần mô phỏng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`N = 4`Và`K = 3`, chúng tôi có`2K = 6 >= 4`Và`K >= N-1`. Chi nhánh cuối cùng được áp dụng. 

| N | K | Tình trạng | A | B | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 3 |`K >= N-1`| 1 | 3 | Mạnh ăn cả hai | 

Đến lúc 0, con cá sấu khỏe mạnh bắt được mảnh nặng 1kg. Nó kết thúc ở thời điểm 1, trước lần ném thứ hai ở thời điểm 3. Khi đó cả hai con cá sấu đều được tự do nên con cá sấu mạnh mẽ sẽ giành chiến thắng và bắt được mảnh 3 kg. Số tiền là 4 và 0, tạo ra chênh lệch 4, điều này không thể tránh khỏi đối với trường hợp này`K`. 

Mẫu chính thức sử dụng`1 3`, do đó cấu trúc này cũng tái tạo đầu ra mẫu. 

### Mẫu 2 

cho`N = 4`Và`K = 1`, chúng tôi có`2K = 2 < 4`, do đó nhánh đầu tiên được áp dụng. 

| N | K | Tình trạng | A | B | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 1 |`2K < N`| 2 | 2 | Mạnh được A, yếu được B | 

Con cá sấu mạnh mẽ bắt được mảnh nặng 2 kg đầu tiên ở thời điểm 0 và sẽ kết thúc ở thời điểm 2. Mảnh thứ hai đến vào thời điểm 1, trong khi con cá sấu mạnh mẽ vẫn đang chiếm giữ. Con cá sấu yếu đuối được tự do nên nó bắt được miếng đó. 

Mỗi con cá sấu nhận được 2 kg, nên sự khác biệt là bằng không. Vì số 0 là chênh lệch nhỏ nhất có thể nên cách xây dựng này là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép tính số học và phép so sánh được thực hiện. | 
| Không gian | O(1) | Chỉ một`N`,`K`,`A`, Và`B`được lưu trữ. | 

Thuật toán không phụ thuộc vào độ lớn của`N`ngoại trừ thông qua số học số nguyên, do đó thậm chí`N = 10^9`Và`K = 10^9`yêu cầu khối lượng công việc tương tự như bài kiểm tra nhỏ nhất. Nó dễ dàng phù hợp với giới hạn thời gian một giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# The tested solution logic is kept here as a function so that
# each assertion can run independently.

import sys
import io

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        N, K = map(int, sys.stdin.readline().split())

        if 2 * K < N:
            A = N // 2
            B = N // 2
        elif K < N - 1:
            A = K + 1
            B = N - K - 1
        else:
            A = 1
            B = N - 1

        return f"{A} {B}"
    finally:
        sys.stdin = old_stdin

# Provided samples
assert solve_case("4 3\n") == "1 3", "sample 1"
assert solve_case("4 1\n") == "2 2", "sample 2"

# Minimum-size input
assert solve_case("2 1\n") == "1 1", "minimum N"

# Balanced construction
assert solve_case("10 1\n") == "5 5", "zero-difference case"

# Exact boundary K = N/2
assert solve_case("6 3\n") == "4 2", "K = N/2"

# Exact boundary K = N-1
assert solve_case("10 9\n") == "1 9", "K = N-1"

# Maximum-size values
assert solve_case("1000000000 1000000000\n") == "1 999999999", "maximum values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`|`1 1`| Tối thiểu có thể`N`, bao gồm cả chi nhánh nơi`K = N-1`. | 
|`10 1`|`5 5`| Một cấu trúc không khác biệt khi`K < N/2`. | 
|`6 3`|`4 2`| Ranh giới nghiêm ngặt`K = N/2`, nơi mà các phần bằng nhau không còn tác dụng nữa. | 
|`10 9`|`1 9`| Ranh giới nghiêm ngặt`K = N-1`, Ở đâu`K+1`sẽ làm`B`không. | 
|`1000000000 1000000000`|`1 999999999`| Giá trị đầu vào tối đa và số học số nguyên lớn. | 

## Vỏ cạnh 

cho`N = 4, K = 1`, thuật toán lấy`A = B = 2`. Miếng đầu tiên được ăn từ thời điểm 0 đến thời điểm 2, trong khi miếng thứ hai đến ở thời điểm 1. Do đó, cá sấu mạnh đang bận và cá sấu yếu ăn miếng thứ hai. Kết quả là`2 2`, cho tổng số bằng nhau. 

Vì`N = 4, K = 2`, đẳng thức tại ranh giới ăn uống sẽ làm thay đổi kết quả. Với`A = B = 2`, con cá sấu khỏe kết thúc đúng lúc quân thứ hai được ném ra nên nó cũng bắt được quân đó. Thay vào đó, thuật toán sẽ chọn`A = 3, B = 1`. Cá sấu mạnh vẫn bận rộn cho đến thời điểm thứ 3, trong khi cá sấu yếu bắt được mảnh 1 kg ở thời điểm 2. Kết quả chênh lệch là 2 và không thể có chênh lệch dương nhỏ hơn vì bất kỳ công trình hai mảnh nào trao mảnh thứ hai cho cá sấu yếu đều yêu cầu`A > 2`. 

Vì`N = 4, K = 3`, chúng tôi đang ở trong`K >= N-1`nhánh và đầu ra`1 3`. Con cá sấu mạnh mẽ hoàn thành miếng đầu tiên ở thời điểm 1, vì vậy cả hai con cá sấu đều được tự do ở thời điểm 3. Con cá sấu mạnh mẽ sẽ chiến thắng trong lần thử đồng thời và ăn luôn miếng thứ hai. Vì mọi mảnh đầu tiên có thể có kích thước tối đa`N-1 = 3`, không miếng đầu tiên nào có thể khiến con cá sấu mạnh mẽ bận rộn cho đến lần thứ 3. Do đó, con cá sấu yếu không thể nhận được miếng thịt nào, tạo nên sự khác biệt`N`không thể tránh khỏi.
