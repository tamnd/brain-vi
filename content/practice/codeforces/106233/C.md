---
title: "CF 106233C - \u0418\u043b\u043b\u044e\u0437\u0438\u044f \u0441\u0443\u043c\u043c\u044b"
description: "Chúng tôi có một thợ làm bánh với số lượng bột nhào hạn chế và một số loại nhân. Một chiếc bánh có nhân cụ thể sẽ tiêu tốn một lượng bột và một lượng nhân đó và mang lại một lợi nhuận nhất định. Người làm bánh cũng có thể làm những chiếc bánh đơn giản chỉ tiêu tốn bột."
date: "2026-06-25T07:02:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106233
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106233
solve_time_s: 39
verified: true
draft: false
---

[CF 106233C - \u0418\u043b\u043b\u044e\u0437\u0438\u044f \u0441\u0443\u043c\u043c\u044b](https://codeforces.com/problemset/problem/106233/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một thợ làm bánh với số lượng bột nhào hạn chế và một số loại nhân. Một chiếc bánh có nhân cụ thể sẽ tiêu tốn một lượng bột và một lượng nhân đó và mang lại một lợi nhuận nhất định. Người làm bánh cũng có thể làm những chiếc bánh đơn giản chỉ tiêu tốn bột. Nhiệm vụ là chọn số lượng bánh mỗi loại để nướng sao cho tổng lợi nhuận lớn nhất có thể. 

Đầu vào mô tả số lượng bột có sẵn, số lượng nhân, chi phí và lợi nhuận của một chiếc bánh mì thường và đối với mỗi loại nhân, số lượng nhân có sẵn, số lượng nhân cần thiết cho mỗi chiếc bánh, số lượng bột cần thiết cho mỗi chiếc bánh và lợi nhuận. Sản lượng là số tiền tối đa có thể kiếm được. 

Giới hạn bột nhiều nhất là 1000, điều này làm thay đổi cách chúng ta nghĩ về vấn đề. Trạng thái được xác định bằng lượng bột còn lại là đủ nhỏ để duy trì. Số lượng loại nhân cũng ít, nhiều nhất là 10 nên việc xử lý riêng từng loại là khả thi. Một giải pháp thử mọi số lượng bánh có thể có cho mọi loại nhân sẽ phát triển quá nhanh vì sự kết hợp tăng lên gấp bội, trong khi giải pháp gần với O(m * n * thứ gì đó nhỏ) thì thoải mái. 

Một sai lầm phổ biến là chỉ chọn loại nhân có lợi nhuận cao nhất trên mỗi gam bột. Điều này không thành công vì chất trám cũng có nguồn lực hạn chế. Ví dụ, hãy xem xét:```
10 2 2 1
7 3 2 100
12 3 1 10
```Sự lựa chọn tốt nhất là không chỉ sử dụng lần đổ đầy đầu tiên, vì nó sẽ hết. Câu trả lời đúng là`241`, sử dụng hai búi tóc loại thứ nhất, bốn búi tóc loại thứ hai và một búi tóc trơn. Một sự lựa chọn tỷ lệ tham lam có thể khiến khối bột có giá trị không được sử dụng. 

Một trường hợp khác là khi có nhân nhưng không có giá trị sử dụng so với bánh thường. Ví dụ:```
100 1 25 50
15 5 20 10
```Đầu ra đúng là:```
200
```Một giải pháp bất cẩn có thể tốn bột làm nhân vì nó có sẵn, nhưng bốn chiếc bánh trơn sẽ mang lại nhiều lợi nhuận hơn. 

Trường hợp cạnh thứ ba là khi lượng nhân không chia hết cho số lượng cần thiết cho một chiếc bánh. Ví dụ:```
5 1 3 10
4 3 2 100
```Chỉ có thể làm được một chiếc bánh nhân vì phần nhân là nguồn lực hạn chế. Phần điền còn lại không thể được sử dụng. Đầu ra đúng là`110`, một bánh nhân và một bánh thường. Một vòng lặp chỉ kiểm tra giới hạn bột có thể đánh giá quá cao câu trả lời. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp là quyết định làm bao nhiêu chiếc bánh cho mỗi loại nhân. Đối với mỗi loại, chúng tôi thử tất cả các số đếm có thể có từ 0 đến số lượng được cho phép bởi nhân và bột. Sau khi sửa tất cả các lựa chọn nhân, phần bột còn lại sẽ được chuyển thành bánh đơn giản. Điều này đúng vì mọi kế hoạch nướng bánh có thể đều được xem xét. 

Vấn đề là số lượng kết hợp. Mặc dù mỗi loại điền chỉ có số lượng lựa chọn hạn chế, nhưng việc kết hợp 10 loại sẽ tạo ra một không gian tìm kiếm lớn. Nếu mỗi loại cho phép khoảng 100 lựa chọn, lực lượng vũ phu sẽ khám phá xung quanh`100^10`sự kết hợp, đó là điều không thể. 

Quan sát hữu ích là nguồn lực duy nhất được chia sẻ giữa tất cả các loại bánh là bột nhào. Tài nguyên điền chỉ ảnh hưởng đến loại điền hiện tại. Vì giới hạn bột nhỏ nên chúng tôi có thể tích trữ lợi nhuận tốt nhất có thể đạt được cho mọi lượng bột đã sử dụng có thể. 

Đối với một loại nhân, chúng ta có thể coi việc làm một chiếc bánh bao như một món đồ trong ba lô có giới hạn. Chúng ta biết số lượng bánh tối đa của loại đó và mỗi chiếc bánh có chi phí bột và lợi nhuận cố định. Chúng tôi cập nhật một mảng lập trình động trong đó`dp[x]`nghĩa là lợi nhuận tối đa sau khi sử dụng chính xác`x`gram bột. Sau khi xử lý mọi loại nhân, phần bột còn lại sẽ được làm nhân bằng bánh mì trơn một cách tối ưu. 

Lực lượng vũ phu hoạt động vì nó liệt kê tất cả các kế hoạch hợp lệ, nhưng nó thất bại vì nó bỏ qua rằng nhiều kế hoạch có cùng số lượng bột đã sử dụng. Phương pháp lập trình động hợp nhất tất cả các kế hoạch như vậy vào một trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(sản phẩm của mọi lựa chọn) | O(1) | Quá chậm | 
| Tối ưu | O(m * n * maxBunsPerType) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc loại bột có sẵn và tất cả các loại bánh. Lưu trữ riêng thông tin về bánh đơn giản vì thông tin này sẽ được áp dụng sau khi tất cả các bánh bao được xem xét. 
2. Tạo mảng lập trình động có kích thước`n + 1`. Giá trị tại vị trí`i`đại diện cho lợi nhuận tối đa sau khi chi tiêu chính xác`i`gram bột. Ban đầu, chi tiêu bằng 0 sẽ không mang lại lợi nhuận. 
3. Xử lý từng loại nhân một. Đối với loại này, hãy tính số lượng bánh tối đa có thể có được từ phần nhân có sẵn. Số lượng thực tế bị giới hạn bởi cả lượng nhân và giới hạn bột. 
4. Hãy thử mọi số lượng bánh có thể có của loại nhân hiện tại. Đối với mỗi lần sử dụng bột trước đó, hãy cập nhật trạng thái mới bằng cách thêm số lượng bánh đã chọn. Điều này có tác dụng vì phần nhân hiện tại không phụ thuộc vào các phần nhân khác ngoại trừ phần nhân qua bột. 
5. Sau khi tất cả các phần nhân đã được xử lý, hãy lặp lại từng lượng bột có thể đã được sử dụng. Thêm lợi nhuận từ việc làm bánh đơn giản với số bột còn lại và giữ câu trả lời tối đa. 

Lý do điều này đúng là vì sau khi xử lý một số loại nhân, trạng thái lập trình động mang lại lợi nhuận tốt nhất có thể cho mỗi lượng bột bỏ ra. Khi thêm một kiểu điền mới, mọi đóng góp có thể có của kiểu đó đều được xem xét, do đó, bất biến vẫn đúng. Sau khi xử lý xong tất cả các phần nhân, quyết định duy nhất còn lại là sử dụng phần bột còn sót lại như thế nào và bánh mì trơn không có hạn chế nào khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, c0, d0 = map(int, input().split())

    types = []
    for _ in range(m):
        a, b, c, d = map(int, input().split())
        types.append((a, b, c, d))

    dp = [0] * (n + 1)

    for a, b, c, d in types:
        limit = min(a // b, n // c)
        ndp = dp[:]

        for dough_used in range(n + 1):
            if dp[dough_used] < 0:
                continue
            for cnt in range(1, limit + 1):
                new_dough = dough_used + cnt * c
                if new_dough > n:
                    break
                value = dp[dough_used] + cnt * d
                if value > ndp[new_dough]:
                    ndp[new_dough] = value

        dp = ndp

    ans = 0
    for used in range(n + 1):
        plain = (n - used) // c0 * d0
        if dp[used] + plain > ans:
            ans = dp[used] + plain

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng`dp`lưu trữ kết quả tốt nhất cho từng lượng bột có thể đã được cam kết. Nó bắt đầu chỉ với kế hoạch nướng trống có sẵn. 

Đối với mỗi lần điền, biến`limit`ngăn chặn những chuyển đổi không thể thực hiện được. Một chiếc bánh chỉ có thể làm được nếu có đủ nhân và đủ bột. Mảng đã sao chép`ndp`được sử dụng vì tất cả các lựa chọn mới cho phần điền hiện tại phải dựa trên bộ phần điền trước đó, chứ không phải dựa trên các quyết định đã sử dụng cùng một phần điền trong cùng một lần lặp. 

Vòng lặp cuối cùng xử lý bánh đơn giản. Nhà nước đã tính toán tất cả các loại bánh nhân nên số bột còn lại chỉ có thể dùng để làm bánh bao thường. Số nguyên trong Python tránh được vấn đề tràn vì câu trả lời tối đa vẫn nhỏ, nhưng việc sử dụng số học số nguyên sẽ xử lý tất cả các giá trị trung gian một cách tự nhiên. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
10 2 2 1
7 3 2 100
12 3 1 10
```Các trạng thái quan trọng sau khi xử lý chất trám có thể được theo dõi như sau. 

| Bước | Đã xử lý điền | Bột đã qua sử dụng | Lợi nhuận tốt nhất | 
| --- | --- | --- | --- | 
| Ban đầu | không | 0 | 0 | 
| Sau khi điền 1 | loại 1 | 2 | 100 | 
| Sau khi điền 1 | loại 1 | 4 | 200 | 
| Sau khi điền 2 | loại 2 | 6 | 240 | 

Trạng thái tốt nhất là sử dụng 6 gram bột cho những chiếc bánh có nhân. Còn lại 4 gram nên có thể làm được hai chiếc bánh đơn giản, cộng thêm một khoản lợi nhuận. Câu trả lời cuối cùng là`241`. 

Đối với mẫu thứ hai:```
100 1 25 50
15 5 20 10
```Việc trám mang lại rất ít lợi nhuận nên trạng thái tối ưu là không sử dụng trám. 

| Bước | Đã xử lý điền | Bột đã qua sử dụng | Lợi nhuận tốt nhất | 
| --- | --- | --- | --- | 
| Ban đầu | không | 0 | 0 | 
| Sau khi điền 1 | loại 1 | 0 | 0 | 
| Cuối cùng | bánh trơn | 100 | 200 | 

Dấu vết cho thấy giai đoạn lập trình động không buộc chúng ta phải sử dụng mọi tài nguyên có sẵn. Phần bột còn lại chỉ được sử dụng ở nơi mang lại lợi nhuận tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n * số bánh trung bình có thể có) | Đối với mỗi lần nhân, chúng tôi thử tất cả các trạng thái bột và số lượng có thể có | 
| Không gian | O(n) | Chỉ các trạng thái bột hiện tại mới được lưu trữ | 

Lượng bột tối đa chỉ là 1000 và có tối đa 10 nhân nên số lần chuyển tiếp dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    n, m, c0, d0 = map(int, data().split())
    types = []
    for _ in range(m):
        a, b, c, d = map(int, data().split())
        types.append((a, b, c, d))

    dp = [0] * (n + 1)

    for a, b, c, d in types:
        limit = min(a // b, n // c)
        ndp = dp[:]
        for i in range(n + 1):
            for cnt in range(1, limit + 1):
                if i + cnt * c > n:
                    break
                ndp[i + cnt * c] = max(ndp[i + cnt * c], dp[i] + cnt * d)
        dp = ndp

    ans = 0
    for i in range(n + 1):
        ans = max(ans, dp[i] + (n - i) // c0 * d0)

    sys.stdin = old
    return str(ans) + "\n"

assert run("""10 2 2 1
7 3 2 100
12 3 1 10
""") == "241\n"

assert run("""100 1 25 50
15 5 20 10
""") == "200\n"

assert run("""5 1 3 10
4 3 2 100
""") == "110\n"

assert run("""1 1 1 5
1 1 1 7
""") == "7\n"

assert run("""20 1 5 10
100 1 5 10
""") == "40\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên | 241 | Trộn nhiều loại trám | 
| Mẫu thứ hai | 200 | Bỏ qua việc trám răng không có lợi | 
|`5 1 3 10 / 4 3 2 100`| 110 | Giới hạn làm đầy không khớp với giới hạn bột | 
|`1 1 1 5 / 1 1 1 7`| 7 | Ranh giới bột tối thiểu | 
|`20 1 5 10 / 100 1 5 10`| 40 | Lựa chọn lợi nhuận ngang bằng | 

## Vỏ cạnh 

Đối với trường hợp điền đầy hạn chế:```
5 1 3 10
4 3 2 100
```Thuật toán tính toán rằng có thể tạo ra nhiều nhất một chiếc bánh nhân vì`4 // 3 = 1`. Quá trình chuyển đổi tạo ra trạng thái sử dụng 2 gam bột và 100 lợi nhuận. 3 gram còn lại tạo thành một chiếc bánh mì đơn giản, cho kết quả cuối cùng`110`. 

Đối với trường hợp bánh trơn thì tốt hơn:```
100 1 25 50
15 5 20 10
```Quá trình chuyển đổi nhân bánh chỉ có thể tạo ra một chiếc bánh nhân có thể có, nhưng mọi lựa chọn như vậy đều để lại quá ít giá trị so với việc sử dụng bột cho bánh mì thông thường. Lần quét cuối cùng so sánh tất cả các trạng thái với lợi nhuận bun đơn giản được thêm vào, vì vậy trạng thái điền trống sẽ thắng với`200`. 

Đối với trường hợp tài nguyên hỗn hợp:```
10 2 2 1
7 3 2 100
12 3 1 10
```Các trạng thái lập trình động kết hợp hai kiểu điền mà không cần nhớ lượng điền còn lại từ các kiểu trước đó. Mỗi lần điền được xử lý một lần với giới hạn riêng, vì vậy trạng thái cuối cùng thể hiện mọi kết hợp hợp lệ.
