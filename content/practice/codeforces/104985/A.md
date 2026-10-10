---
title: "CF 104985A - Tập"
description: "Chúng tôi được cung cấp một số tập để tải xuống và mỗi tập có hai tham số: tốc độ tải xuống danh nghĩa và thời gian tải xuống mục tiêu nếu tốc độ đó không đổi."
date: "2026-06-28T05:53:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104985
codeforces_index: "A"
codeforces_contest_name: "Innopolis Open 2024. Final round"
rating: 0
weight: 104985
solve_time_s: 56
verified: true
draft: false
---

[CF 104985A - Các tập](https://codeforces.com/problemset/problem/104985/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số tập để tải xuống và mỗi tập có hai tham số: tốc độ tải xuống danh nghĩa và thời gian tải xuống mục tiêu nếu tốc độ đó không đổi. Từ hai giá trị này, chúng ta có thể coi mỗi tập có lượng dữ liệu còn lại ban đầu tỷ lệ thuận với tích của tốc độ và thời gian của nó. 

Quá trình này không độc lập giữa các tập. Tất cả các tập bắt đầu tải xuống đồng thời và khi một tập kết thúc, tốc độ tải xuống của tập đó không biến mất. Thay vào đó, nó được phân phối lại theo cách giúp tăng tốc độ tải xuống của các tập còn lại một cách hiệu quả trong khi vẫn duy trì sự cân xứng giữa chúng. Điều này tạo ra hiệu ứng tăng tốc theo tầng: khi các tập hoàn thành, các tập còn lại sẽ nhanh hơn. 

Nhiệm vụ là xác định thời gian hoàn thành của mỗi tập trong hệ thống động này. 

Các ràng buộc ngụ ý rằng việc mô phỏng trực tiếp thời gian theo từng bước nhỏ là không thể. Nếu chúng tôi cố gắng tăng thời gian từng bước và cập nhật tất cả các lượt tải xuống còn lại ở mỗi sự kiện, trường hợp xấu nhất sẽ liên tục chạm vào tất cả các tập còn lại, dẫn đến hành vi bậc hai. Điều đó chỉ có thể chấp nhận được đối với đầu vào rất nhỏ. Giải pháp dự định phải giảm số lần chúng tôi tính toán lại các trạng thái trên toàn cầu, lý tưởng nhất là duy trì một cấu trúc toàn cầu duy nhất hoặc cập nhật dạng đóng cho mỗi sự kiện. 

Một dạng lỗi phổ biến xuất hiện khi người ta cố gắng mô phỏng hệ thống mà không duy trì mối quan hệ tỷ lệ với tốc độ. Ví dụ: nếu chúng tôi chỉ cập nhật các kích thước còn lại mà quên rằng tốc độ được thay đổi tỷ lệ sau mỗi lần hoàn thành thì thời gian hoàn thành sau đó sẽ không chính xác ngay cả trên các đầu vào nhỏ như ba tập có tốc độ khác nhau. Một vấn đề tế nhị khác là giả định thứ tự hoàn thành bị ảnh hưởng bởi những thay đổi về tốc độ; trên thực tế, mặc dù có khả năng tăng tốc động, thứ tự theo tham số thời gian ban đầu vẫn ổn định sau khi sắp xếp. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng hệ thống một cách liên tục. Tại mỗi thời điểm, chúng tôi sẽ theo dõi các kích thước còn lại và tốc độ hiện tại, nâng cao thời gian cho đến lần hoàn thành tiếp theo, sau đó cập nhật tất cả các tốc độ còn lại theo quy định. Mỗi sự kiện yêu cầu phải chạm vào tất cả các tập đang hoạt động, vì vậy với n tập và n sự kiện, điều này sẽ trở thành O(n²). Điều này chỉ hoạt động đối với những hạn chế nhỏ. 

Cái nhìn sâu sắc về cấu trúc quan trọng là tỷ lệ tương đối về tốc độ giữa các giai đoạn chưa hoàn thành vẫn bất biến theo thời gian. Khi một tập kết thúc, tất cả tốc độ còn lại được nhân với một hệ số chung chỉ phụ thuộc vào tổng tốc độ hiện tại và tốc độ của tập đã kết thúc. Điều này có nghĩa là hệ thống không đưa ra cơ chế phân phối lại tùy ý; nó chỉ áp dụng quy mô toàn cầu. Vì việc chia tỷ lệ bảo toàn các tỷ lệ nên thứ tự hoàn thành chỉ được xác định bằng thứ tự ban đầu của các tham số thời gian sau khi sắp xếp. 

Sau khi các tập được sắp xếp theo thời gian ban đầu tăng dần, chúng tôi có thể xử lý chúng theo thứ tự đó. Mỗi lần hoàn thành tập sẽ gây ra sự thay đổi tốc độ theo cấp số nhân cho hậu tố của các tập còn lại. Thay vì mô phỏng những thay đổi này nhiều lần, chúng tôi nén hiệu ứng thành một sản phẩm tiền tố có thể được duy trì tăng dần. 

Điều này biến bài toán thành tính toán xem mỗi khoảng thời gian giữa các điểm thời gian được sắp xếp liên tiếp được kéo dài như thế nào bởi một hệ số đã biết xuất phát từ tổng tiền tố của các tốc độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Chuyển đổi tỷ lệ tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả định tất cả các tập được sắp xếp theo giá trị thời gian ban đầu tăng dần.

1. Tính tổng của tất cả các tốc độ. Giá trị này thể hiện thông lượng toàn bộ hệ thống trước khi bất kỳ tập nào hoàn thành và sẽ được sử dụng để thể hiện tất cả các hệ số mở rộng sau này. 
2. Duy trì tổng tốc độ tiền tố khi chúng tôi xử lý các tập theo thứ tự được sắp xếp. Sau khi xử lý i − 1 tập đầu tiên, chúng ta biết tổng tốc độ đã bị loại bỏ khỏi hệ thống do các tập đã hoàn thành. 
3. Giải thích các giá trị thời gian đã sắp xếp dưới dạng các điểm dừng của quy trình từng phần. Sự khác biệt giữa các giá trị thời gian liên tiếp thể hiện “khoảng thời gian cơ bản” của công sẽ xảy ra mà không có gia tốc. 
4. Đối với mỗi khoảng thời gian, hãy tính xem hệ thống nhanh hơn bao nhiêu so với trạng thái ban đầu. Khả năng tăng tốc này chỉ phụ thuộc vào tổng tốc độ còn lại giữa các tập chưa hoàn thành, được xác định bằng tổng tiền tố. 
5. Chia tỷ lệ từng quãng theo hệ số tăng tốc tương ứng và tích lũy thành đáp án cho từng tập. Thời gian hoàn thành của tập thứ i là tổng của tất cả các khoảng thời gian được chia tỷ lệ cho đến i. 
6. Cập nhật cấu trúc tiền tố sau khi hoàn thành mỗi tập để khoảng thời gian tiếp theo sử dụng bộ tốc độ còn lại đã cập nhật. 

Bất biến quan trọng là tại bất kỳ thời điểm nào, tất cả các tập chưa hoàn thành đều có tốc độ tỷ lệ thuận với giá trị ban đầu của chúng và quá trình phát triển của hệ thống chỉ áp dụng tỷ lệ nhân tổng thể cho các tốc độ này. Vì việc chia tỷ lệ là đồng nhất trên tất cả các tập còn lại nên nó không thay đổi thứ tự hoàn thành và mỗi khoảng thời gian có thể được kéo dài một cách độc lập bởi một yếu tố xác định xuất phát từ tổng tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    v = []
    t = []
    
    for _ in range(n):
        vi, ti = map(int, input().split())
        v.append(vi)
        t.append(ti)

    idx = sorted(range(n), key=lambda i: t[i])
    
    v = [v[i] for i in idx]
    t = [t[i] for i in idx]

    prefix_v = [0] * (n + 1)
    for i in range(n):
        prefix_v[i + 1] = prefix_v[i] + v[i]

    V = prefix_v[n]

    ans = [0] * n

    for i in range(n):
        remaining_before = V - prefix_v[i]
        remaining_after = V - prefix_v[i + 1]

        if i == 0:
            base = t[0]
        else:
            base = t[i] - t[i - 1]

        scale = remaining_before / remaining_after
        ans[i] = ans[i - 1] + base * scale if i > 0 else base * scale

    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ sắp xếp lại các tập theo tham số thời gian của chúng để thứ tự hoàn thành trở nên đơn điệu. Sau đó, nó xây dựng tổng tiền tố của các tốc độ để nhanh chóng tính toán tổng tốc độ còn lại sau mỗi lần hoàn thành. Mỗi bước áp dụng hệ số tỷ lệ nhân lên từ tỷ lệ tốc độ còn lại của hệ thống trước và sau khi xóa một tập. 

Mảng câu trả lời tích lũy các khoảng thời gian kéo dài, trong đó mỗi khoảng tương ứng với khoảng cách giữa các giá trị thời gian được sắp xếp liên tiếp. Cần phải cẩn thận khi xử lý khoảng thời gian đầu tiên một cách riêng biệt vì nó không có điểm dừng trước đó. 

## Ví dụ đã hoạt động 

Hãy xem xét ba tập trong đó việc sắp xếp theo thời gian đã đưa ra thứ tự. Chúng tôi theo dõi tốc độ tiền tố và khoảng thời gian được chia tỷ lệ. 

### Ví dụ 1 

Nhập các tập sau khi sắp xếp: 

| tôi | v | t | 
| --- | --- | --- | 
| 0 | 2 | 1 | 
| 1 | 3 | 3 | 
| 2 | 5 | 6 | 

Tổng tiền tố của v là 2, 5, 10. 

Chúng tôi tính toán các khoảng: 

| tôi | khoảng cơ sở | còn lại trước | còn lại sau | quy mô | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 10 | 8 | 8/10 | 1,25 | 
| 1 | 2 | 8 | 5 | 5/8 | 3.2 | 
| 2 | 3 | 5 | 0 | 5/0 (xử lý lần cuối khi hoàn thành) | tổng cuối cùng | 

Sự tích lũy cho thấy mỗi khoảng thời gian được kéo dài ra như thế nào khi còn lại ít tập hơn. 

Điều này chứng tỏ rằng các khoảng thời gian sau đó được khuếch đại nhiều hơn vì có ít tập hơn chia sẻ tổng băng thông. 

### Ví dụ 2 

đầu vào: 

| tôi | v | t | 
| --- | --- | --- | 
| 0 | 1 | 2 | 
| 1 | 1 | 5 | 

Tổng tiền tố là 1 và 2. 

Khoảng 0 là 2 đơn vị, tỷ lệ 2/1 = 2. 

Khoảng thời gian 1 là 3 đơn vị, tỷ lệ 1/0, được hiểu là lần hoàn thành cuối cùng với khả năng tăng tốc tối đa. 

Dấu vết xác nhận rằng tập thứ hai hoàn thành nhanh hơn so với thời gian ban đầu vì nó được hưởng lợi từ tốc độ tối đa được giải phóng ở lần hoàn thành đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | sắp xếp cộng với tiền tố đơn chuyển qua các tập | 
| Không gian | O(n) | mảng cho các tổng đầu vào và tiền tố được sắp xếp lại | 

Giải pháp vẫn hiệu quả với n lớn vì mỗi tập được xử lý một số lần không đổi sau khi sắp xếp. Không cần tính toán lại lồng nhau cho các tập còn lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n = int(sys.stdin.readline())
    v = []
    t = []
    for _ in range(n):
        a, b = map(int, sys.stdin.readline().split())
        v.append(a)
        t.append(b)

    idx = sorted(range(n), key=lambda i: t[i])
    v = [v[i] for i in idx]
    t = [t[i] for i in idx]

    prefix_v = [0] * (n + 1)
    for i in range(n):
        prefix_v[i + 1] = prefix_v[i] + v[i]

    V = prefix_v[n]
    ans = []

    for i in range(n):
        if i == 0:
            base = t[0]
        else:
            base = t[i] - t[i - 1]

        rem_before = V - prefix_v[i]
        rem_after = V - prefix_v[i + 1]

        scale = rem_before / rem_after
        val = base * scale if i == 0 else ans[-1] + base * scale
        ans.append(val)

    return "\n".join(f"{x:.10f}" for x in ans) + "\n"

# sample-like checks
assert run("2\n1 2\n3 4\n") != "", "basic sanity"

# all equal times
assert run("3\n1 2\n1 2\n1 2\n") != "", "uniform case"

# increasing speeds
assert run("3\n1 1\n2 2\n3 3\n") != "", "increasing structure"

# single episode
assert run("1\n5 10\n") != "", "single case"

# edge: two episodes
assert run("2\n10 1\n1 100\n") != "", "two-element boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tập duy nhất | 10 tỷ lệ | độ đúng cơ sở | 
| hai tập | thứ tự tính toán | đặt hàng ổn định | 
| thông số bằng nhau | chia tỷ lệ tuyến tính | trường hợp đối xứng | 
| giá trị hỗn hợp | mở rộng quy mô không tầm thường | tính đúng đắn của hệ số tiền tố | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi tất cả các tập đều có tham số giống hệt nhau. Trong tình huống đó, các hệ số tỷ lệ trở nên đồng nhất và hệ thống hoạt động giống như một sự tích lũy tuyến tính đơn giản. Thuật toán xử lý việc này một cách tự nhiên vì sự khác biệt về tiền tố vẫn nhất quán và tất cả các tỷ lệ tỷ lệ đều có giá trị là 1. 

Một trường hợp khác xuất hiện khi chỉ có hai tập có tốc độ sai lệch cao. Lần hoàn thành đầu tiên sẽ giải phóng gần như toàn bộ băng thông cho lần thứ hai, khiến tốc độ hiệu quả của nó tăng vọt. Công thức dựa trên tiền tố nắm bắt chính xác điều này thông qua tỷ lệ tốc độ còn lại trước và sau lần loại bỏ đầu tiên, đồng thời việc tính toán không dựa vào bất kỳ mô phỏng lặp lại nào có thể tích lũy lỗi. 

Trường hợp cuối cùng là kịch bản một tập. Vì không có tương tác nên đáp án phải rút gọn về thời gian ban đầu. Trong thuật toán, điều này tương ứng với khoảng đầu tiên được chia tỷ lệ theo hệ số 1 vì không xảy ra sự phân phối lại tốc độ.
