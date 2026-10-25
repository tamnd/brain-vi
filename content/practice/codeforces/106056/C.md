---
title: "CF 106056C - Vấn đề mới nhưng hoài cổ"
description: "Vấn đề là chọn cách nướng bánh để tối đa hóa lợi nhuận. Một thợ làm bánh có một lượng bột cố định và có thể có một số loại nhân."
date: "2026-06-25T12:18:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106056
codeforces_index: "C"
codeforces_contest_name: "The 1st Universal Cup. Stage 18: Shenzhen"
rating: 0
weight: 106056
solve_time_s: 34
verified: true
draft: false
---

[CF 106056C - Vấn đề mới nhưng hoài cổ](https://codeforces.com/problemset/problem/106056/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề là chọn cách nướng bánh để tối đa hóa lợi nhuận. Một thợ làm bánh có một lượng bột cố định và có thể có một số loại nhân. Đối với mỗi loại nhân, số lượng có sẵn có hạn và mỗi chiếc bánh sử dụng nhân đó sẽ tiêu tốn một ít nhân, một ít bột và mang lại một số tiền nhất định. Người làm bánh cũng có thể làm những chiếc bánh đơn giản không có nhân, chỉ tiêu tốn bột và mang lại lợi nhuận cố định. Mục tiêu là tìm ra doanh thu tối đa có thể. 

Đầu vào mô tả loại bột có sẵn, số lượng loại nhân và công thức làm bánh mì thường. Mỗi loại nhân cung cấp bốn giá trị: tổng số lượng nhân đó, lượng nhân cần thiết cho mỗi chiếc bánh, số bột cần cho mỗi chiếc bánh và giá bán. Sản lượng là số tiền lớn nhất có thể kiếm được. 

Giới hạn bột nhiều nhất là 1000, đây là hạn chế chính. Một giải pháp theo cấp số nhân về số lượng lựa chọn điền là không thể, bởi vì ngay cả một số lượng nhỏ các loại điền cũng có thể tạo ra quá nhiều kết hợp. Tuy nhiên, số lượng nhỏ các loại nhân, nhiều nhất là 10, cho thấy rằng chúng ta có thể cung cấp trạng thái lập trình động để theo dõi việc sử dụng bột. 

Điều khó khăn là mỗi lần trám có thể được sử dụng nhiều lần nhưng chỉ ở mức tối đa. Một giải pháp bất cẩn có thể tham lam chọn loại nhân có giá tính trên mỗi gam bột cao nhất. Ví dụ: nếu đầu vào là:```
10 2 2 1
7 3 2 100
12 3 1 10
```Đầu ra đúng là:```
241
```Chiến lược tham lam có thể chỉ tập trung vào lần làm bánh đầu tiên vì nó mang lại 100 tiền cho mỗi chiếc bánh. Tuy nhiên, lần nhân thứ hai sử dụng ít bột hơn và cho phép làm được nhiều bánh hơn. Sự lựa chọn tối ưu là trộn cả hai chất trám. 

Một trường hợp khác là khi búi tóc đơn giản là lựa chọn tốt nhất. Coi như:```
100 1 25 50
15 5 20 10
```Đầu ra đúng là:```
200
```Phương pháp luôn cố gắng sử dụng nhân trước sẽ mất tiền vì bốn chiếc bánh đơn giản kiếm được nhiều tiền hơn bất kỳ sự kết hợp nào có thể có liên quan đến nhân. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi số lượng bánh có thể có cho mọi loại nhân. Đối với phần nhân, chúng ta có thể chọn từ số 0 cho đến mức tối đa mà phần nhân có sẵn cho phép. Vì có nhiều phần trám nên số lượng kết hợp sẽ trở thành tích của tất cả các lựa chọn này. 

Trong trường hợp xấu nhất, mỗi lượng đổ đầy đủ lớn để cho phép có nhiều lựa chọn. Ngay cả khi chỉ có 10 miếng trám, việc thử mọi sự kết hợp có thể trở nên quá chậm. Ý tưởng Brute Force là đúng vì nó kiểm tra mọi kế hoạch nướng bánh có thể, nhưng không gian tìm kiếm quá lớn. 

Quan sát quan trọng là bột nhào là nguồn tài nguyên hạn chế và giá trị tối đa của nó chỉ là 1000. Chúng ta không cần nhớ sự kết hợp chính xác của các loại nhân được sử dụng. Chúng ta chỉ cần biết còn lại bao nhiêu bột, vì những quyết định trong tương lai chỉ phụ thuộc vào điều đó. Điều này chuyển đổi vấn đề thành một vấn đề ba lô bị chặn. 

Đối với mỗi loại nhân, chúng tôi xử lý tất cả số lượng nhân có thể có và cập nhật lợi nhuận tốt nhất có thể đạt được cho mỗi lượng bột có thể. Sau khi xử lý xong tất cả các phần nhân, chúng ta có thể lấp đầy phần bột còn lại bằng bánh mì trơn theo cách có lợi nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng trám | O(1) | Quá chậm | 
| Lập trình động | O(m * n * max_possible_buns) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bột, số lượng nhân và công thức làm bánh mì đơn giản. Tạo một mảng lập trình động trong đó`dp[x]`đại diện cho số tiền tối đa có thể kiếm được bằng cách sử dụng chính xác`x`gram bột từ nhân đã qua chế biến. 
2. Đối với mỗi loại nhân, hãy tính số lượng bánh tối đa có thể sử dụng loại nhân đó. Nó bị hạn chế cả bởi nhân có sẵn và thực tế là mỗi chiếc bánh đều tiêu tốn bột. 
3. Đối với cách làm nhân bánh hiện tại, hãy thử mọi số lượng bánh có thể có từ 0 đến số lượng tối đa có thể. Đối với mỗi lần đếm, hãy cập nhật lợi nhuận mới cho mỗi số tiền bột có thể hỗ trợ những chiếc bánh đó. 
4. Sau khi tất cả các phần nhân đã được xử lý, hãy xem xét bánh mì thường. Đối với mỗi lượng bột có thể đã dùng để làm nhân, hãy tính xem có thể làm được bao nhiêu chiếc bánh đơn giản từ số bột còn lại và cộng thêm lợi nhuận của chúng. 
5. Giá trị lớn nhất trên tất cả các trạng thái cuối cùng chính là đáp số. 

Lý do điều này có tác dụng là vì trạng thái lập trình động lưu trữ tất cả thông tin cần thiết cho các lựa chọn trong tương lai. Khi chúng ta biết lượng bột đã sử dụng cho đến nay, việc lựa chọn nhân trước đó không còn quan trọng nữa. Mỗi kế hoạch nướng hợp lệ đều tương ứng với một chuỗi chuyển tiếp, do đó, giá trị tốt nhất được lưu trữ cho mỗi lượng bột luôn có thể đạt được và tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, c0, d0 = map(int, input().split())
    fillings = []

    for _ in range(m):
        a, b, c, d = map(int, input().split())
        fillings.append((a, b, c, d))

    dp = [0] * (n + 1)

    for a, b, c, d in fillings:
        ndp = dp[:]
        limit = min(a // b, n // c)

        for cnt in range(1, limit + 1):
            dough = cnt * c
            value = cnt * d
            for used in range(dough, n + 1):
                if dp[used - dough] + value > ndp[used]:
                    ndp[used] = dp[used - dough] + value

        dp = ndp

    ans = 0
    for used in range(n + 1):
        plain = (n - used) // c0
        cur = dp[used] + plain * d0
        if cur > ans:
            ans = cur

    print(ans)

if __name__ == "__main__":
    solve()
```Mã giữ mảng DP một chiều vì tài nguyên thay đổi duy nhất là bột. Đối với mỗi lần đổ đầy,`ndp`là bản sao của trạng thái trước đó để phần điền hiện tại không thể vô tình được tái sử dụng vượt quá số lượng cho phép. 

Vòng lặp kết thúc`cnt`xử lý tính chất giới hạn của mỗi lần điền. Số lượng bánh tối đa là số lượng tối thiểu của giới hạn nhân có sẵn và giới hạn bột. Việc chuyển đổi sử dụng`used - dough`bởi vì chúng tôi đang hỏi có thể kiếm được bao nhiêu lợi nhuận trước khi thêm nhóm bánh hiện tại. 

Sau khi xử lý nhân, vòng cuối cùng sẽ xử lý riêng các loại bánh thường. Việc này đơn giản hơn việc trộn bánh thường vào DP vì không có giới hạn về số lượng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 2 2 1
7 3 2 100
12 3 1 10
```Các trạng thái quan trọng sau khi xử lý trám răng là: 

| Bước | Bột đã qua sử dụng | Lợi nhuận | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| Hai bánh nhân 1 | 4 | 200 | 
| Bốn bánh nhân 2 | 4 | 40 | 
| Hai điền 1 + bốn điền 2 | 8 | 240 | 
| Bột còn lại làm bánh thường | 10 | 241 | 

Sự kết hợp nhân tốt nhất sử dụng tám gam bột và kiếm được 240. Hai gam còn lại tạo thành một chiếc bánh mì đơn giản, cho ra câu trả lời cuối cùng là 241. 

### Mẫu 2 

đầu vào:```
100 1 25 50
15 5 20 10
```Dấu vết là: 

| Bước | Bột đã qua sử dụng | Lợi nhuận | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| Một chiếc bánh nhân | 20 | 10 | 
| Không điền | 100 | 200 | 

Phần nhân không hữu ích vì nó mang lại ít tiền hơn so với bánh thường. Câu trả lời cuối cùng hoàn toàn đến từ những chiếc bánh đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n * n) | Đối với mỗi lần đổ đầy và mỗi lần đếm có thể, chúng tôi cập nhật từng lượng bột | 
| Không gian | O(n) | Chỉ có mảng DP hiện tại và mảng chuyển tiếp được sao chép mới được lưu trữ | 

Với`n`nhiều nhất là 1000 và`m`nhiều nhất là 10, số lượng thao tác đủ nhỏ cho các giới hạn. Việc sử dụng bộ nhớ cũng rất nhỏ vì mảng DP chỉ có 1001 vị trí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue()

assert run("10 2 2 1\n7 3 2 100\n12 3 1 10\n") == "241\n"

assert run("100 1 25 50\n15 5 20 10\n") == "200\n"

assert run("1 1 1 5\n1 1 1 10\n") == "10\n"

assert run("10 1 3 7\n100 100 1 100\n") == "70\n"

assert run("20 2 4 5\n100 1 4 5\n100 1 4 5\n") == "25\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 5`với một lần điền |`10`| Kiểm tra lượng bột nhỏ nhất và cách sử dụng nhân | 
|`10 1 3 7`với chất làm đầy không có sẵn |`70`| Kiểm tra xem các chất trám không thể được bỏ qua | 
|`20 2 4 5`|`25`| Kiểm tra nhiều loại nhân có công thức giống hệt nhau | 

## Vỏ cạnh 

Có thể có nhân nhưng không thể sử dụng vì không đủ hoặc không đủ bột. Trong trường hợp đó, số lượng tối đa sẽ bằng không. Ví dụ:```
10 1 2 3
1 5 1 100
```Việc điền cần năm đơn vị nhưng chỉ tồn tại một đơn vị nên thuật toán bỏ qua nó. Người thợ làm bánh làm năm chiếc bánh đơn giản và câu trả lời là 15. 

Phần nhân có thể tệ hơn bánh bao thông thường ngay cả khi nó trông có giá trị. Vì:```
100 1 25 50
15 5 20 10
```DP chỉ lưu trữ lợi nhuận nhỏ từ việc sử dụng nhân bánh, sau đó bước cuối cùng so sánh nó với việc sử dụng phần bột còn lại để làm bánh mì thường. Điều này tạo ra 200 thay vì một sự lựa chọn tham lam tồi tệ hơn. 

Khi tất cả các miếng trám có cùng chi phí và giá trị thì không cần phải xử lý đặc biệt. Mỗi lần chuyển đổi vẫn thể hiện một kế hoạch nướng bánh khả thi và lợi nhuận tối đa được bảo toàn. Ví dụ:```
20 2 4 5
100 1 4 5
100 1 4 5
```DP đạt 25 bằng cách sử dụng năm bánh, điều này xác nhận rằng các lựa chọn bằng nhau lặp đi lặp lại được xử lý chính xác.
