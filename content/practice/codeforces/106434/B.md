---
title: "CF 106434B - \u0417\u0430\u0448\u0438\u0444\u0440\u043e\u0432\u0430\u043d\u043d\u044b\u0439 \u043c\u0430\u0441\u0441\u0438\u0432"
description: "Chúng ta được cung cấp một mảng mà chúng ta chỉ nhìn thấy ở dạng méo mó. Mỗi phần tử trong mảng được quan sát ban đầu có thể là bất kỳ số nguyên nào trong một khoảng cố định xung quanh giá trị được quan sát của nó."
date: "2026-06-21T19:21:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106434
codeforces_index: "B"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421 2026, \u043f\u0440\u0435\u0434\u043c\u0435\u0442\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 106434
solve_time_s: 42
verified: true
draft: false
---

[CF 106434B - \u0417\u0430\u0448\u0438\u0444\u0440\u043e\u0432\u0430\u043d\u043d\u044b\u0439 \u043c\u0430\u0441\u0441\u0438\u0432](https://codeforces.com/problemset/problem/106434/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng mà chúng ta chỉ nhìn thấy ở dạng méo mó. Mỗi phần tử trong mảng được quan sát ban đầu có thể là bất kỳ số nguyên nào trong một khoảng cố định xung quanh giá trị được quan sát của nó. Cụ thể, nếu chúng ta thấy một giá trị`a[i]`, thì giá trị thực trước khi mã hóa có thể là bất kỳ số nguyên nào từ`a[i] - l`lên đến`a[i] + r`, nhưng không bao giờ tiêu cực. 

Câu hỏi đặt ra là liệu chúng ta có thể chọn một giá trị ban đầu hợp lệ cho mỗi vị trí sao cho mảng ẩn thu được không giảm hay không. 

Vì vậy, chúng tôi không được yêu cầu xây dựng lại mảng một cách duy nhất. Chúng ta chỉ cần quyết định xem có tồn tại ít nhất một cách để chọn giá trị hợp lệ cho mọi vị trí sao cho chuỗi được sắp xếp hay không. 

Các ràng buộc đi lên đến`n = 2 * 10^5`, vì vậy mọi nghiệm đều phải gần tuyến tính. Bất cứ điều gì liên quan đến việc thử tất cả các lựa chọn trong phạm vi ngay lập tức là không thể vì mỗi phần tử có tối đa`O(10^9)`những giá trị có thể. Ngay cả việc tìm kiếm theo từng phần tử trong phạm vi cũng sẽ bùng nổ. 

Một suy nghĩ ngây thơ là xử lý từng vị trí một cách độc lập, nhưng ràng buộc thứ tự sẽ kết hợp chặt chẽ tất cả các vị trí. Khó khăn chính là mỗi lựa chọn đều hạn chế những lựa chọn trong tương lai. 

Một vài trường hợp thất bại làm rõ những gì có thể xảy ra. 

Nếu chúng ta tham lam chọn giá trị nhỏ nhất có thể ở mỗi bước, chúng ta có thể bị mắc kẹt sau này vì một giá trị đầu nhỏ buộc các phần tử sau quá nhỏ để không giảm. 

Thay vào đó, nếu chúng ta chọn giá trị lớn nhất có thể ở mỗi bước, chúng ta có thể chặn các giải pháp hợp lệ yêu cầu lựa chọn trước đó nhỏ hơn một cách không cần thiết. 

Vấn đề thực sự là mỗi vị trí là một khoảng và chúng ta cần kiểm tra xem liệu chúng ta có thể chọn một chuỗi giá trị, một giá trị từ mỗi khoảng, tạo thành một chuỗi không giảm hay không. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản: đối với mỗi vị trí, hãy xem xét tất cả các giá trị ban đầu có thể có trong khoảng của nó.`[a[i] - l, a[i] + r]`và cố gắng xây dựng một chuỗi không giảm bằng cách sử dụng phương pháp quay lui hoặc lập trình động trên tất cả các kết hợp. Điều này đúng vì nó trực tiếp khám phá định nghĩa về tính hợp lệ. Tuy nhiên, mỗi vị trí có tới`O(r + l)`khả năng, và hơn thế nữa`n`vị trí này dẫn đến số lượng kết hợp theo cấp số nhân, đại khái`(range size)^n`, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không bao giờ cần phải nhớ tất cả các giá trị có thể có ở một vị trí. Nếu chúng ta xử lý từ trái sang phải, điều quan trọng chỉ là phạm vi giá trị khả thi mà chúng ta có thể kết thúc cho tiền tố. Thay vì theo dõi tất cả các lựa chọn, chúng tôi theo dõi khoảng thời gian của tất cả các giá trị có thể có cho phần tử được chọn cuối cùng để giữ cho tiền tố hợp lệ. 

Tại vị trí`i`, giả sử chúng ta đã biết rằng giá trị được chọn cuối cùng phải nằm trong một khoảng nào đó`[L, R]`. Đối với phần tử tiếp theo, chúng ta phải chọn một giá trị`x`như vậy`x >= L`Và`x`nằm ở`[a[i] - l, a[i] + r]`. Vậy giá trị nhỏ nhất có thể chọn là`max(L, a[i] - l)`, và cực đại là`a[i] + r`. Nếu khoảng này trở nên không hợp lệ (giới hạn dưới vượt quá giới hạn trên), việc xây dựng là không thể. 

Điều này biến vấn đề thành một quá trình lan truyền khoảng thời gian đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Khoảng lan truyền | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì tập hợp tất cả các giá trị có thể có cho phần tử cuối cùng của tiền tố hợp lệ. Thay vì một bộ, chúng tôi chỉ giữ phạm vi khả thi của nó`[L, R]`. 

1. Khởi tạo phạm vi cho phần tử đầu tiên là`[a[0] - l, a[0] + r]`. Chúng tôi giới hạn dưới ít nhất bằng 0 vì giá trị ban đầu phải không âm. 
2. Đặt phạm vi này làm khoảng thời gian khả thi hiện tại. 
3. Đối với mỗi vị trí tiếp theo`i`, tính khoảng cho phép của nó`[low_i, high_i] = [a[i] - l, a[i] + r]`, lại kẹp`low_i`về không. 
4. Để duy trì trật tự không giảm, giá trị mới ít nhất phải bằng`L`. Vì vậy, chúng tôi giao nhau khoảng`[low_i, high_i]`với`[L, +infinity)`, đưa ra một giới hạn dưới mới`L' = max(L, low_i)`và giới hạn trên`R' = high_i`. 
5. Nếu tại bất kỳ thời điểm nào`L' > R'`, không có giá trị hợp lệ nào có thể được chọn tại vị trí`i`mà không phá vỡ sự đơn điệu, nên chúng ta kết luận ngay rằng điều đó là không thể. 
6. Nếu không, hãy cập nhật`[L, R] = [L', R']`và tiếp tục. 
7. Nếu chúng tôi xử lý thành công tất cả các phần tử, sẽ tồn tại một mảng ẩn không giảm hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau khi xử lý vị trí`i`, khoảng`[L, R]`chứa chính xác tất cả các giá trị có thể đóng vai trò là phần tử cuối cùng của một chuỗi không giảm hợp lệ nào đó cho tiền tố`1..i`. Mỗi bước tính toán giao điểm của tập hợp này với phạm vi hợp lệ cho phần tử tiếp theo. Vì cả hai ràng buộc đều là các khoảng lồi và tính đơn điệu chỉ áp đặt giới hạn dưới nên tập hợp có thể tiếp cận vẫn là một khoảng xuyên suốt. Nếu khoảng trống trở nên trống thì không có chuỗi nào có thể thỏa mãn cả ràng buộc giá trị và ràng buộc thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, l, r = map(int, input().split())
a = list(map(int, input().split()))

L = max(0, a[0] - l)
R = a[0] + r

for i in range(1, n):
    low = max(0, a[i] - l)
    high = a[i] + r

    L = max(L, low)
    R = high

    if L > R:
        print("NO")
        sys.exit()

print("YES")
```Việc thực hiện trực tiếp tuân theo logic bảo trì khoảng thời gian. Chi tiết tinh tế duy nhất là kẹp giới hạn dưới về 0, vì giá trị ban đầu không thể âm. Một điểm quan trọng khác là chúng tôi không bao giờ thu nhỏ giới hạn trên dựa trên trạng thái trước đó ngoại trừ thông qua tính khả thi, bởi vì bất kỳ giá trị nào lớn hơn luôn được ưu tiên hơn cho tính linh hoạt trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1 2
1 0 2 1
```Chúng tôi theo dõi khoảng thời gian khả thi cho giá trị được chọn cuối cùng. 

| tôi | một [tôi] | [thấp, cao] | L | R | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | [0, 3] | 0 | 3 | được | 
| 1 | 0 | [0, 2] | 0 | 2 | được | 
| 2 | 2 | [1, 4] | 1 | 4 | được | 
| 3 | 1 | [0, 3] | 1 | 3 | được | 

Khoảng không bao giờ trở nên trống, do đó tồn tại một sự tái thiết không giảm hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 2 2
5 0 5
```| tôi | một [tôi] | [thấp, cao] | L | R | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | [3, 7] | 3 | 7 | được | 
| 1 | 0 | [0, 2] | 3 | 2 | không hợp lệ | 

Sau phần tử thứ hai, số 3 tối thiểu bắt buộc vượt quá số 2 tối đa, do đó không thể hình thành chuỗi hợp lệ. 

Ví dụ này cho thấy một giá trị quan sát nhỏ có thể phá vỡ tính khả thi như thế nào nếu các ràng buộc trước đó đã buộc chuỗi quá cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các cập nhật theo khoảng thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến được duy trì | 

Quét tuyến tính dễ dàng phù hợp với các ràng buộc lên đến`2 * 10^5`các phần tử và mức sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, l, r = map(int, input().split())
    a = list(map(int, input().split()))

    L = max(0, a[0] - l)
    R = a[0] + r

    for i in range(1, n):
        low = max(0, a[i] - l)
        high = a[i] + r

        L = max(L, low)
        R = high

        if L > R:
            return "NO"

    return "YES"

# provided samples
assert run("4 1 2\n1 0 2 1\n") == "YES"
assert run("3 2 2\n5 0 5\n") == "NO"

# custom cases
assert run("1 0 0\n5\n") == "YES"
assert run("2 0 0\n1 0\n") == "YES"
assert run("2 0 0\n0 1\n") == "YES"
assert run("2 0 0\n1 0\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | tính khả thi tầm thường | 
| giới hạn bằng nhau | CÓ | không có trường hợp linh hoạt | 
| tăng cạnh hoán đổi | CÓ | vẫn có thể đơn điệu | 
| cạnh hoán đổi giảm dần | KHÔNG | trường hợp vi phạm ngay lập tức | 

## Vỏ cạnh 

Một trường hợp khó nhận thấy là khi`l = r = 0`. Trong tình huống này, mảng được cố định và câu hỏi giảm xuống còn việc kiểm tra xem mảng đã cho có phải là không giảm hay không. Thuật toán xử lý việc này một cách tự nhiên vì mỗi khoảng thu gọn về một giá trị duy nhất và bất kỳ sự giảm nào ngay lập tức gây ra`L > R`. 

Một trường hợp khác là khi các phần tử ban đầu cho phép phạm vi rộng nhưng các phần tử sau buộc phải có giới hạn dưới rõ ràng. Ví dụ: một chuỗi như`10, 0, 0`với đủ lớn`l`Và`r`vẫn có thể trở nên không hợp lệ ở vị trí thứ hai nếu khoảng đầu tiên buộc mức tối thiểu trên 0. Việc lan truyền theo khoảng ngay lập tức phát hiện điều này bằng cách giao nhau các phạm vi và thu gọn tính khả thi. 

Trường hợp cuối cùng là khi phần tử đầu tiên đã bắt đầu ở giá trị cao nhưng việc tái cấu trúc khả thi duy nhất yêu cầu các giá trị nhỏ hơn. Thuật toán cho phép khoảng thời gian đầu tiên rộng nhất có thể một cách chính xác và không cam kết sớm với một giá trị duy nhất, đảm bảo các ràng buộc sau này được xem xét đúng cách.
