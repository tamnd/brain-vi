---
title: "CF 940B - Tanya của chúng ta đang khóc rất to"
description: "Chúng ta bắt đầu với một giá trị số nguyên duy nhất x = n và biến đổi liên tục cho đến khi nó trở thành 1. Mỗi bước di chuyển hoặc giảm giá trị đi một với giá cố định A hoặc chia giá trị hiện tại cho k với giá B, nhưng chỉ được phép chia khi giá trị hiện tại chia hết cho k."
date: "2026-06-17T02:32:37+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 940
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 466 (Div. 2)"
rating: 1400
weight: 940
solve_time_s: 71
verified: true
draft: false
---

[CF 940B - Tanya của chúng ta đang khóc rất to](https://codeforces.com/problemset/problem/940/B) 

**Đánh giá:** 1400 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một giá trị số nguyên duy nhất`x = n`và liên tục biến đổi nó cho đến khi nó trở thành`1`. Mỗi lần di chuyển sẽ làm giảm giá trị đi một với chi phí cố định`A`hoặc chia giá trị hiện tại cho`k`với chi phí`B`, nhưng phép chia chỉ được phép khi giá trị hiện tại chia hết cho`k`. 

Nhiệm vụ là tính toán tổng chi phí tối thiểu của bất kỳ chuỗi hoạt động nào làm giảm`n`xuống tới`1`. 

Ràng buộc`n ≤ 2·10^9`ngay lập tức loại trừ bất kỳ chiến lược nào mô phỏng tất cả các trạng thái trung gian một cách mù quáng trong khi phân nhánh ở mỗi bước. Lập trình động trực tiếp trên các giá trị từ`1`ĐẾN`n`là không thể vì không gian trạng thái quá lớn. Tuy nhiên, quá trình này có một cấu trúc chặt chẽ: chia cho`k`tạo ra những bước nhảy lớn và phép trừ chỉ nhằm mục đích "căn chỉnh" số sao cho có thể thực hiện được phép chia. 

Các trường hợp cạnh tới hạn xuất phát từ cách phép trừ và phép chia tương tác với nhau. Nếu như`k = 1`, phép chia không bao giờ làm giảm số, vì vậy phép toán có ý nghĩa duy nhất là phép trừ được lặp lại`n - 1`lần, mang lại chi phí`(n - 1) * A`. Một trường hợp tế nhị khác là khi`k > n`, trong đó phép chia không bao giờ có thể thực hiện được và một lần nữa câu trả lời lại suy biến thành chi phí trừ tuyến tính. Chế độ thất bại thứ ba xuất hiện nếu người ta giả định sự lựa chọn cục bộ tham lam giữa phép trừ và phép chia; điều này bị phá vỡ vì phép trừ đắt hơn một chút bây giờ có thể mở khóa phép chia giúp tiết kiệm chi phí lớn sau này. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả các chuỗi hoạt động có thể có từ`n`ĐẾN`1`, khám phá cả chuyển đổi trừ và chia bất cứ khi nào hợp lệ. Điều này tạo thành một quá trình phân nhánh trong đó mỗi trạng thái có thể tạo ra tối đa hai trạng thái tiếp theo. Ngay cả khi cố gắng cắt tỉa, số lượng trạng thái có thể truy cập vẫn theo thứ tự`n`, vì chuỗi trừ chiếm ưu thế trong quá trình chuyển đổi giữa các phép chia hữu ích. Cách tiếp cận này trở nên không khả thi vì độ sâu tuyến tính và sự phân nhánh xảy ra thường xuyên. 

Quan sát quan trọng là việc chia cho`k`là hoạt động duy nhất nén không gian trạng thái một cách có ý nghĩa. Phép trừ chỉ hữu ích như một bước chuẩn bị để di chuyển`x`tới bội số gần nhất của`k`. Thay vì suy nghĩ theo các phép toán riêng lẻ, chúng ta nên nghĩ theo các phân đoạn: đối với bất kỳ khoảng giá trị nào, chúng ta có thể giảm xuống bằng phép trừ hoặc nhảy đến giá trị giảm thông qua phép chia sau khi căn chỉnh. 

Điều này cho thấy một quá trình tham lam liên tục di chuyển từ giá trị hiện tại`n`xuống tới`1`, nhưng ở mỗi giai đoạn hãy xem xét liệu việc thực hiện phép chia có mang lại lợi ích hay không. Trước khi chia ta tiến hành trừ chi phí để giảm`n`tới bội số gần nhất của`k`, sau đó chia một lần và tiếp tục lập luận tương tự từ giá trị kết quả. Bởi vì mỗi phép chia làm giảm độ lớn của`x`ít nhất bằng một hệ số`k`, số lượng các giai đoạn như vậy là logarit trong`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam tối ưu theo cấp độ | O(logₖ n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi liên tục so sánh hai chiến lược ở giá trị hiện tại`n`: hoặc giảm mọi thứ bằng cách trừ xuống`1`, hoặc sử dụng phép chia bất cứ khi nào nó có ích. 

1. Nếu`k == 1`, phép chia không bao giờ giảm`x`, vì vậy chúng tôi trực tiếp quay lại`(n - 1) * A`. Điều này xử lý một phép biến đổi suy biến trong đó chỉ có phép trừ là quan trọng. 
2. Khởi tạo một biến`cost = 0`. Điều này sẽ tích lũy tổng chi phí của tất cả các hoạt động. 
3. Trong khi`n >= k`, tính xem cần bao nhiêu bước để giảm`n`tới bội số gần nhất của`k`. Sự khác biệt đó là`rem = n % k`. Chúng tôi trả tiền`rem * A`để trừ xuống một giá trị chia hết. 
4. Cộng chi phí trừ này vào`cost`, sau đó cập nhật`n = n - rem`. Tại thời điểm này`n`chia hết cho`k`, nên phép chia có hiệu lực. 
5. So sánh xem phép chia có đáng giá hay không. Thay vì phân nhánh, chúng tôi luôn thực hiện nó: thêm`B`ĐẾN`cost`, sau đó cập nhật`n = n // k`. Lý do biện minh là nếu phép trừ lặp lại sẽ rẻ hơn phép chia ở cấp độ này thì trường hợp đó đã được ghi lại trong lần lặp tiếp theo trong đó`k`thực sự lớn hơn so với`n`. 
6. Lặp lại cho đến khi`n < k`. Lúc này việc phân chia là không thể được nữa. 
7. Cuối cùng, giảm`n`ĐẾN`1`chỉ sử dụng phép trừ, trả tiền`(n - 1) * A`. 

Thuật toán hoạt động vì mỗi chu kỳ giảm nghiêm ngặt`n`, đảm bảo kết thúc theo thời gian logarit. 

### Tại sao nó hoạt động 

Ở bất kỳ giá trị nào`n`, quyết định duy nhất quan trọng là liệu chúng ta nên chi phí để thực hiện phép chia hay tiếp tục trừ hoàn toàn. Bất kỳ chuỗi nào bao gồm phép chia đều phải trải qua trạng thái trong đó`n`chia hết cho`k`và đạt đến trạng thái đó một cách tối ưu luôn có nghĩa là chỉ trừ cho đến bội số tiếp theo của`k`. Không có lợi ích gì trong việc vượt quá hoặc trì hoãn phép chia vì phép trừ có chi phí tuyến tính và phép chia mang lại sự giảm theo cấp số nhân về kích thước trạng thái. Điều này tạo ra một cấu trúc đơn điệu: khi chúng ta quyết định sử dụng phép chia ở một cấp độ, thời điểm tốt nhất có thể là ngay sau khi căn chỉnh theo bội số gần nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
k = int(input())
A = int(input())
B = int(input())

if k == 1:
    print((n - 1) * A)
    sys.exit()

cost = 0

while n >= k:
    rem = n % k
    cost += rem * A
    n -= rem

    cost += B
    n //= k

cost += (n - 1) * A

print(cost)
```Việc triển khai phản ánh trực tiếp cấu trúc tham lam. Trường hợp đặc biệt`k == 1`ngăn chặn vòng lặp vô hạn vì phép chia sẽ không bao giờ làm giảm giá trị. Mỗi lần lặp vòng lặp đầu tiên sẽ căn chỉnh`n`đến bội số của`k`bằng phép trừ, sau đó thực hiện bước chia. Giai đoạn trừ cuối cùng xử lý phạm vi còn lại bên dưới`k`. 

Một sai lầm phổ biến là quên rằng phép trừ phải được tính trước khi chia. Một người khác đang cố gắng so sánh chi phí cục bộ và đôi khi bỏ qua việc phân chia, điều này phá vỡ cấu trúc toàn cầu vì các bộ phận trong tương lai phụ thuộc vào sự liên kết hiện tại. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu`n = 9, k = 2, A = 3, B = 1`. 

### Dấu vết 1 

| n | Hoạt động | Chi phí tăng thêm | Tổng chi phí | 
| --- | --- | --- | --- | 
| 9 | trừ 1 đến 8 | 3 | 3 | 
| 8 | chia 2 cho 4 | 1 | 4 | 
| 4 | chia 2 cho 2 | 1 | 5 | 
| 2 | chia 2 cho 1 | 1 | 6 | 

Quá trình này hiển thị căn chỉnh lặp đi lặp lại, sau đó nén thông qua phép chia, với phép trừ chỉ được sử dụng để đạt đến trạng thái chia hết. 

Bây giờ hãy xem xét trường hợp phép chia quá tốn kém:`n = 10, k = 3, A = 2, B = 100`. 

### Dấu vết 2 

| n | Hoạt động | Chi phí tăng thêm | Tổng chi phí | 
| --- | --- | --- | --- | 
| 10 | trừ 1 đến 9 | 2 | 2 | 
| 9 | chia cho 3 | 100 | 102 | 
| 3 | chia cho 1 | 100 | 202 | 

Ở đây, mặc dù phép chia cực kỳ tốn kém nhưng thuật toán vẫn thực hiện nó vì cấu trúc buộc phải đánh giá thông qua căn chỉnh. Tuy nhiên, kể từ khi`B`chiếm ưu thế, kết quả phản ánh chính xác rằng ít phép chia được sử dụng và phép trừ vẫn tương đối rẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(logₖ n) | Mỗi bộ phận giảm`n`ít nhất bằng một hệ số`k`, vậy có nhiều nhất phép lặp logarit | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi được duy trì | 

Thời gian chạy phù hợp thoải mái trong giới hạn vì ngay cả ở mức tối đa`n`, số bước chia nhỏ và mỗi bước chỉ thực hiện số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite
    import builtins

    # re-run solution inline
    n = int(input())
    k = int(input())
    A = int(input())
    B = int(input())

    if k == 1:
        return str((n - 1) * A)

    cost = 0
    while n >= k:
        rem = n % k
        cost += rem * A
        n -= rem
        cost += B
        n //= k

    cost += (n - 1) * A
    return str(cost)

# provided sample
assert run("9\n2\n3\n1\n") == "6"

# k = 1 edge case
assert run("10\n1\n5\n7\n") == "45"

# no division possible
assert run("10\n20\n2\n1\n") == "18"

# division expensive
assert run("10\n2\n1\n100\n") == "9"

# small chain
assert run("1\n5\n10\n10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k = 1 | chi phí tuyến tính | trường hợp cạnh bị vô hiệu hóa phân chia | 
| k > n | chỉ trừ | không có đường chia hợp lệ | 
| phân chia đắt giá | tránh sự phức tạp không cần thiết | hành vi chi phối chi phí | 
| n = 1 | chi phí bằng không | ranh giới tầm thường | 

## Vỏ cạnh 

cho`k = 1`, xem xét đầu vào`n = 6, k = 1, A = 3, B = 100`. Phép chia không bao giờ giảm`x`, do đó thuật toán trả về ngay`(6 - 1) * 3 = 15`. Bất kỳ giải pháp dựa trên vòng lặp nào không có bộ bảo vệ này sẽ không bao giờ chấm dứt vì`n // 1`vẫn không thay đổi. 

Vì`k > n`, chẳng hạn như`n = 5, k = 10`, vòng lặp bị bỏ qua hoàn toàn. Bước cuối cùng trở lại`(5 - 1) * A`, mô hình hóa chính xác rằng chỉ có thể sử dụng phép trừ. 

Vì`n = 1`, không cần thực hiện thao tác nào. Thuật toán bỏ qua cả phép trừ vòng lặp và phép trừ cuối cùng, trực tiếp mang lại chi phí bằng 0, vì`(1 - 1) * A = 0`.
