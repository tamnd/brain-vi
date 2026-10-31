---
title: "CF 106350I - Hoán đổi(Mohamed, !Mohamed)"
description: "Sự cố mô tả một hàng hộp. Một số hộp chứa Mohamed, được biểu thị bằng 1, trong khi các hộp trống được biểu thị bằng 0. Mohamed muốn chọn một hộp làm điểm gặp cuối cùng và di chuyển mọi Mohamed hiện có trong một hộp khác sang hộp đã chọn đó."
date: "2026-06-25T08:07:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106350
codeforces_index: "I"
codeforces_contest_name: "Zaglol Contest - FCDS level 1 contest 2026"
rating: 0
weight: 106350
solve_time_s: 31
verified: true
draft: false
---

[CF 106350I - Hoán đổi(Mohamed, !Mohamed)](https://codeforces.com/problemset/problem/106350/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Sự cố mô tả một hàng hộp. Một số hộp chứa Mohamed, đại diện bởi`1`, trong khi các hộp trống được biểu thị bằng`0`. Mohamed muốn chọn một ô làm điểm gặp gỡ cuối cùng và di chuyển mọi Mohamed hiện đang ở trong một ô khác sang ô đã chọn đó. Di chuyển Mohamed từ hộp này sang hộp liền kề sẽ tốn một lần hoán đổi, do đó di chuyển một khoảng cách là`d`chi phí hộp`d`hoạt động. Nhiệm vụ là tìm số lần hoán đổi tối thiểu cần thiết trên tất cả các hộp đích có thể. 

Đầu vào chứa số lượng hộp và chuỗi nhị phân mô tả trạng thái hiện tại của chúng. Đầu ra là tổng chi phí di chuyển nhỏ nhất có thể. 

Các ràng buộc được thiết kế để loại trừ việc kiểm tra mọi điểm đến và mô phỏng chuyển động nhiều lần. Nếu có`n`hộp và`n`có thể lớn, một giải pháp thử mọi đích đến và quét toàn bộ chuỗi sẽ mất`O(n^2)`hoạt động quá chậm. Chúng ta cần một phương pháp trong đó mỗi hộp chỉ được xử lý một số lần không đổi. 

Các trường hợp chính xảy ra từ các vị trí không có Mohamed và từ các hàng mà tất cả Mohamed đã ở cùng nhau. Việc triển khai bất cẩn có thể cho rằng đích đến phải chứa Mohamed, nhưng đích đến tốt nhất có thể là một ô trống. 

Ví dụ:```
5
00100
```Đầu ra đúng là:```
0
```Việc chọn ô ở giữa đã có Mohamed không cần phải di chuyển. Phương pháp chỉ kiểm tra các vị trí trống hoặc chỉ xem xét việc di chuyển đến các cạnh sẽ thất bại. 

Một ví dụ khác:```
6
100001
```Đầu ra đúng là:```
5
```Việc chọn bất kỳ hộp ở giữa nào sẽ có chi phí lớn hơn, trong khi việc chọn một trong hai điểm cuối sẽ di chuyển một Mohamed năm hộp và để lại hộp còn lại ở đó. Một cách tiếp cận ngây thơ chỉ tính số lượng Mohamed thay vì khoảng cách của họ sẽ bỏ lỡ chi phí thực tế. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là thử mọi hộp đích có thể. Đối với một điểm đến đã chọn`i`, quét toàn bộ chuỗi và bất cứ khi nào một hộp chứa Mohamed, hãy thêm khoảng cách giữa hộp đó và`i`. Điều này đúng vì tổng công việc cần thiết cho một điểm đến chính xác là tổng của tất cả các chuyển động riêng lẻ. 

Tuy nhiên, việc thực hiện điều này cho mọi điểm đến sẽ lặp lại gần như các phép tính giống nhau nhiều lần. Với`n`những điểm đến có thể và`n`các hộp để quét từng cái, trường hợp xấu nhất là`O(n^2)`hoạt động. Đối với một hàng lớn, điều này nhanh chóng trở nên không thể. 

Quan sát chính là các điểm đến lân cận có liên quan với nhau. Nếu chúng ta biết tổng chi phí cho việc xếp mọi người vào ô`i`, chúng ta có thể tính được chi phí cho hộp`i + 1`mà không cần quét lại toàn bộ hàng. 

Khi chúng ta di chuyển đích đến một bước sang bên phải, mọi Mohamed ở phía bên trái sẽ trở nên xa hơn một bước, trong khi mọi Mohamed ở phía bên phải sẽ tiến gần hơn một bước. Nếu chúng ta duy trì số lượng Mohamed ở mỗi bên, chi phí sẽ thay đổi một cách đơn giản. Điều này cho phép chúng tôi tính toán tất cả các điểm đến theo thời gian tuyến tính. 

Phương pháp brute-force hoạt động vì mọi điểm đến có thể đều được đánh giá chính xác, nhưng nó lãng phí thời gian khi tính toán lại các khoảng cách chồng chéo. Việc quan sát xem chi phí thay đổi như thế nào giữa các đích lân cận sẽ làm giảm vấn đề về việc đếm tiền tố và hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm sự đóng góp của tất cả Mohamed khi đích đến là ô đầu tiên. 

Đối với mỗi Mohamed ở vị trí`i`, giá của nó là`i`vì nó phải di chuyển`i`hộp để đạt được vị trí đầu tiên. Tổng hợp các giá trị này sẽ đưa ra câu trả lời ban đầu cho đích đến đầu tiên. 
2. Lưu trữ chi phí ban đầu này và chuẩn bị di chuyển đích từ trái sang phải. 

Trong khi chuyển mục tiêu, chúng ta chỉ cần biết hiện tại có bao nhiêu Mohamed ở bên trái và bên phải. 
3. Di chuyển điểm đến sang bên phải một vị trí mỗi lần. 

Giả sử đích di chuyển từ vị trí`i`ĐẾN`i + 1`. Mỗi Mohamed tại các vị trí`0`bởi vì`i`trở nên xa hơn một bước, làm tăng chi phí bằng số Mohamed ở bên trái. Mỗi Mohamed sau`i`tiến gần hơn một bước, giảm chi phí bằng số Mohamed ở bên phải. 
4. Cập nhật chi phí hiện tại bằng cách sử dụng chênh lệch này và giữ nguyên giá trị tối thiểu được thấy. 

Vì mọi đích đến có thể đều đạt được chính xác một lần nên giá trị nhỏ nhất được ghi lại là câu trả lời tối ưu. 

Tại sao nó hoạt động: 

Điều bất biến là trước khi xử lý một điểm đến, chi phí được lưu trữ chính xác là số lần hoán đổi cần thiết để di chuyển mọi Mohamed đến vị trí đó. Di chuyển điểm đến theo một vị trí sẽ thay đổi khoảng cách của từng cá nhân chính xác bằng một, tăng hoặc giảm tùy thuộc vào việc Mohamed đứng về phía nào. Bởi vì tất cả các khoản đóng góp đều được cập nhật cùng nhau bằng cách sử dụng số lượng của chúng nên mọi chi phí đích được tính toán đều chính xác, do đó mức tối thiểu tìm được là mức tối ưu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    total = 0
    ones = 0

    for i, c in enumerate(s):
        if c == '1':
            total += i
            ones += 1

    ans = total

    left = 0
    right = ones

    current = total

    for i in range(n - 1):
        if s[i] == '1':
            left += 1
        right = ones - left

        current += left - right
        ans = min(ans, current)

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán chi phí nếu đích được chọn là hộp đầu tiên. Một Mohamed tại chỉ số`i`đóng góp chính xác`i`hoán đổi trong tình huống này. 

Các biến`left`Và`right`duy trì số lượng Mohamed trước và sau điểm đến hiện tại trong khi quét từ trái sang phải. Khi đích đến di chuyển một vị trí,`left`nhóm tiến xa hơn một và`right`nhóm ngày càng gần nhau hơn, đó là lý do tại sao bản cập nhật được thực hiện`left - right`. 

Vòng lặp dừng ở`n - 1`vì sau nước đi cuối cùng sẽ không có đích đến tiếp theo để đánh giá. Tất cả số học đều sử dụng số nguyên Python, do đó tổng chuyển động lớn không bị tràn. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
5
00100
```| Điểm đến sau khi xử lý | Mohamed trái | Đúng Mohamed | Chi phí hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 2 | 2 | 
| 1 | 0 | 1 | 1 | 1 | 
| 2 | 1 | 0 | 0 | 0 | 
| 3 | 1 | 0 | 1 | 0 | 
| 4 | 1 | 0 | 2 | 0 | 

Mohamed duy nhất bắt đầu ở trung tâm, vì vậy việc chọn cùng một ô sẽ không chuyển động. Công thức chuyển đổi giảm chi phí một cách chính xác cho đến khi đạt đến vị trí đó và tăng sau đó. 

Một ví dụ khác:```
6
100001
```| Điểm đến sau khi xử lý | Mohamed trái | Đúng Mohamed | Chi phí hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 5 | 5 | 
| 1 | 1 | 1 | 5 | 5 | 
| 2 | 1 | 1 | 5 | 5 | 
| 3 | 1 | 1 | 5 | 5 | 
| 4 | 1 | 1 | 5 | 5 | 
| 5 | 2 | 0 | 5 | 5 | 

Đích tối ưu là bất kỳ điểm cuối hoặc vị trí giữa nào trong trường hợp đối xứng này. Thuật toán đánh giá mọi khả năng và giữ mức tối thiểu như nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuỗi được quét một số lần không đổi. | 
| Không gian | O(1) | Chỉ bộ đếm và câu trả lời hiện tại được lưu trữ. | 

Thuật toán xử lý mỗi hộp một lần, do đó nó chia tỷ lệ tuyến tính theo độ dài hàng và phù hợp với giới hạn dự kiến. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline
    n = int(data())
    s = data().strip()

    total = 0
    ones = 0

    for i, c in enumerate(s):
        if c == '1':
            total += i
            ones += 1

    ans = total
    left = 0
    current = total

    for i in range(n - 1):
        if s[i] == '1':
            left += 1
        right = ones - left
        current += left - right
        ans = min(ans, current)

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert run("5\n00100\n") == "0\n", "single Mohamed already centered"
assert run("6\n100001\n") == "5\n", "symmetric endpoints"
assert run("1\n1\n") == "0\n", "minimum size"
assert run("8\n11111111\n") == "16\n", "all boxes occupied"
assert run("7\n0001000\n") == "0\n", "single Mohamed with many empty boxes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 / 00100`|`0`| Điểm đến đã bị Mohamed chiếm đóng | 
|`6 / 100001`|`5`| Chuyển động đối xứng và chi phí bằng nhau | 
|`1 / 1`|`0`| Đầu vào nhỏ nhất có thể | 
|`8 / 11111111`|`16`| Vụ án dày đặc có nhiều người đóng góp | 
|`7 / 0001000`|`0`| Mohamed đơn và khoảng cách ranh giới | 

## Vỏ cạnh 

cho`00100`, thuật toán bắt đầu với chi phí ban đầu là`2`, bởi vì Mohamed cách ô đầu tiên hai vị trí. Khi điểm đến di chuyển sang phải, bản cập nhật sẽ thay đổi chi phí bằng sự chênh lệch giữa Mohamed ở bên trái và bên phải. Khi điểm đến đến trung tâm, chi phí sẽ trở thành`0`, được ghi lại là câu trả lời. 

Vì`100001`, chi phí ban đầu là`5`. Di chuyển điểm đến về phía giữa làm cho một Mohamed gần hơn và một Mohamed khác ở xa hơn với cùng một lượng như nhau, do đó tổng số vẫn còn`5`. Thuật toán không cho rằng phần giữa luôn tốt hơn và trả về chính xác giá trị tối thiểu. 

Vì`11111111`, mỗi hộp chứa Mohamed. Thuật toán xử lý việc này mà không có trường hợp đặc biệt nào. Mỗi ca thay đổi chi phí theo sự cân bằng của Mohamed ở cả hai bên và mức tối thiểu xảy ra ở gần trung tâm, tạo ra chuyển động tổng thể chính xác. 

Đối với một hộp duy nhất như`1`, các vòng lặp không thực hiện bất kỳ chuyển tiếp nào vì không có đích lân cận. Chi phí ban đầu đã là câu trả lời cuối cùng, giúp tránh được những sai sót nhỏ.
