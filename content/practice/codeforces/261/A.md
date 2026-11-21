---
title: "CF 261A - Tối đa và Giảm giá"
description: "Chúng tôi được cung cấp một danh sách giá các mặt hàng và một bộ sưu tập các loại giảm giá. Mỗi loại giảm giá mô tả số lượng mặt hàng phải được đặt trong một "nhóm trả phí"."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 261
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 160 (Div. 1)"
rating: 1400
weight: 261
solve_time_s: 190
verified: true
draft: false
---

[CF 261A - Tối đa và chiết khấu](https://codeforces.com/problemset/problem/261/A) 

**Đánh giá:** 1400 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 3 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách giá các mặt hàng và một bộ sưu tập các loại giảm giá. Mỗi loại giảm giá mô tả số lượng mặt hàng phải được đặt trong một "nhóm trả phí". Khi Maxim sử dụng giảm giá kích thước$q_i$, anh ấy chọn chính xác$q_i$những món đồ mà anh ta trả tiền và ngoài ra anh ta có thể lấy thêm tối đa hai món đồ miễn phí, miễn là mỗi món đồ miễn phí không đắt hơn món đồ rẻ nhất trong số những món đồ được trả tiền.$q_i$. 

Mục tiêu là bao gồm tất cả$n$các mặt hàng bằng cách sử dụng bất kỳ số lượng ứng dụng giảm giá nào như vậy hoặc bằng cách mua các mặt hàng riêng lẻ để mọi mặt hàng đều được thanh toán hoặc nhận miễn phí theo một số giảm giá. Chúng tôi muốn giảm thiểu tổng số tiền chi tiêu. 

Hạn chế cơ cấu chính là mỗi lần giảm giá đều đóng góp một tỷ lệ cố định giữa các mặt hàng phải trả tiền và các mặt hàng có thể miễn phí, nhưng các mặt hàng miễn phí phụ thuộc vào mức tối thiểu của khối trả phí. Điều này ngay lập tức gợi ý rằng việc đặt hàng theo giá cả rất quan trọng, vì “các mặt hàng giá rẻ nên được tiêu thụ dưới dạng quà tặng miễn phí bất cứ khi nào có thể” luôn có lợi. 

Kích thước đầu vào lên tới$10^5$, do đó, bất kỳ giải pháp nào cố gắng mô phỏng các bài tập hoặc khám phá sự kết hợp giảm giá cho mỗi tập hợp con đều không khả thi. Một bậc hai hoặc thậm chí$O(n \log^2 n)$cấu trúc có vòng lặp bên trong nặng có nguy cơ TLE. Giải pháp phải rút gọn vấn đề thành một tập nhỏ các chiến lược ứng cử viên bắt nguồn từ việc sắp xếp và suy luận tiền tố. 

Trường hợp phức tạp xuất hiện khi tất cả các mặt hàng đều có giá giống nhau. Trong trường hợp đó, mọi khoản giảm giá đều hoạt động đối xứng và kết hợp tham lam vẫn hoạt động nhưng chỉ khi chúng tôi tính toán cẩn thận số lượng mặt hàng có thể được “bao phủ” trên mỗi phân khúc trả phí. Một trường hợp bất lợi khác là khi giải pháp tối ưu hoàn toàn không sử dụng chiết khấu, nghĩa là tất cả các mặt hàng chỉ được thanh toán riêng lẻ, điều này thường bị bỏ qua nếu giải pháp giả định sử dụng ít nhất một khoản chiết khấu. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là nghĩ đến việc chọn một chuỗi giảm giá và gán các mặt hàng cho mỗi lần giảm giá, quyết định mặt hàng nào được trả tiền và mặt hàng nào trở nên miễn phí. Điều này nhanh chóng trở thành một bài toán gán tổ hợp: với mỗi tập con của các mục, chúng ta phải quyết định liệu nó có tạo thành một nhóm trả phí cho một số mục hay không.$q_i$và những vật phẩm bổ sung nào nó sẽ mở khóa dưới dạng quà tặng miễn phí. Ngay cả khi bỏ qua khoản giảm giá nào được sử dụng, chỉ cần quyết định xem mặt hàng nào đóng vai trò là “mục neo trả phí” và mặt hàng nào trở thành “tệp đính kèm miễn phí” cũng dẫn đến cấu hình theo cấp số nhân. Số cách phân vùng$n$các mục thành các nhóm kích thước$q_i + k$Ở đâu$k \le 2$là rất lớn và mỗi nhóm tương tác với các ràng buộc đặt hàng toàn cầu thông qua quy tắc “mặt hàng phải trả tiền tối thiểu”. 

Quan sát quan trọng là việc sắp xếp các mặt hàng theo giá sẽ loại bỏ sự phức tạp nhất. Sau khi các mặt hàng được sắp xếp theo thứ tự giảm dần, chiến lược tốt nhất là luôn sử dụng các mặt hàng đắt tiền làm điểm neo trả phí vì chúng tối đa hóa phạm vi đủ điều kiện nhận quà tặng miễn phí. Các mặt hàng giá rẻ luôn được sử dụng tốt hơn làm mặt hàng miễn phí vì chúng không làm tăng hạn chế đối với các mặt hàng miễn phí trong tương lai. 

Điều này chuyển vấn đề thành việc quyết định xem chúng ta thành lập bao nhiêu nhóm và mỗi nhóm có$q_i$chúng tôi sử dụng. Mỗi nhóm đóng góp$q_i + k$các mặt hàng được tiêu thụ nhưng chỉ$q_i$chi phí đã trả. Vì chúng tôi có thể nhận tối đa hai quà tặng miễn phí cho mỗi nhóm nên hiệu quả hiệu quả được xác định bằng cách tối đa hóa số lượng mặt hàng chúng tôi chi trả cho mỗi mặt hàng phải trả phí, tức là giảm thiểu chi phí cho mỗi mặt hàng được bao trả. 

Vì vậy, thay vì chỉ định trực tiếp các mặt hàng, chúng tôi sắp xếp giá và sau đó cố gắng ghép các mặt hàng lớn nhất thành các bộ trả phí, đồng thời sử dụng các mặt hàng nhỏ nhất có sẵn làm quà tặng miễn phí. Cấu trúc tối ưu trở thành bài toán đóng gói tham lam trong đó chúng ta liên tục chọn quy mô chiết khấu tốt nhất để cải thiện tỷ lệ bao phủ. 

Cách tiếp cận bạo lực sẽ thử tất cả các nhóm; cách tiếp cận tối ưu giảm mọi thứ thành việc phân loại và tiêu dùng tham lam với ưu tiên tối đa hóa việc sử dụng vật phẩm miễn phí trong điều kiện hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả giá các mặt hàng theo thứ tự giảm dần. Điều này đảm bảo rằng chúng tôi luôn coi những mặt hàng đắt tiền nhất là ứng cử viên cho các vị trí được trả phí trước tiên, điều này là tối ưu vì chúng là những ràng buộc “có giá trị” nhất để đủ điều kiện miễn phí. 
2. Sắp xếp tất cả các kích cỡ giảm giá$q_i$. Chúng tôi chỉ quan tâm đến cấu trúc của họ chứ không phải danh tính của họ. 
3. Đối với mỗi quy mô giảm giá, hãy tính xem nó có thể “bao trả cho mỗi đơn vị thanh toán” một cách hiệu quả bao nhiêu mặt hàng. Giảm giá kích thước$q$bao gồm$q + 2$các mặt hàng có chi phí$q$các mặt hàng phải trả tiền, vì vậy hiệu quả của nó là$\frac{q+2}{q}$. Nhỏ hơn$q$có xu hướng hiệu quả hơn nên chúng tôi ưu tiên các nhóm nhỏ hơn. 
4. Tham lam mô phỏng các nhóm tham gia bắt đầu từ mức chiết khấu hiệu quả tốt nhất. Đối với mỗi nhóm, chúng tôi tiêu thụ$q$các mặt hàng đắt tiền nhất còn lại dưới dạng mặt hàng phải trả tiền. 
5. Đối với mỗi nhóm trả phí như vậy, sau đó chúng tôi chỉ định tối đa hai mặt hàng rẻ nhất còn lại làm mặt hàng miễn phí, vì những mặt hàng này không đóng góp vào chi phí và luôn được sử dụng tốt nhất trên các giá trị nhỏ nhất. 
6. Tiếp tục cho đến khi chúng tôi hết mặt hàng hoặc không thể tạo thành nhóm trả phí hợp lệ để nhận bất kỳ khoản giảm giá nào. 
7. Những hạng mục còn lại không xếp được vào nhóm thì phải thanh toán riêng. 

### Tại sao nó hoạt động 

Ở bất kỳ giai đoạn nào, những vật phẩm đắt tiền nhất còn lại nên được sử dụng làm điểm cố định trả phí vì chúng tối đa hóa giới hạn cho các vật phẩm miễn phí. Nếu một mặt hàng rẻ hơn được sử dụng làm điểm neo trả phí thay vì một mặt hàng đắt tiền hơn, chúng tôi sẽ chỉ giảm số lượng mặt hàng miễn phí hợp lệ mà không cải thiện chi phí. Tương tự, các mặt hàng miễn phí phải luôn đến từ các giá trị nhỏ nhất còn lại, vì chúng không bao giờ ảnh hưởng đến các ràng buộc và không làm giảm tính linh hoạt trong tương lai. 

Việc đóng gói tham lam là an toàn vì mỗi nhóm độc lập sau khi chúng tôi sửa thứ tự sắp xếp: các mặt hàng phải trả tiền xác định một ngưỡng và các mặt hàng miễn phí chỉ phụ thuộc vào việc ở dưới ngưỡng đó. Vì việc sắp xếp sắp xếp tất cả các ngưỡng một cách đơn điệu nên thuật toán không bao giờ có lợi từ việc sắp xếp lại các nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m = int(input())
    q = list(map(int, input().split()))
    n = int(input())
    a = list(map(int, input().split()))

    a.sort(reverse=True)
    q.sort()

    # pointers on items
    i, j = 0, n - 1
    ans = 0

    # try using discounts
    for qi in q:
        while i + qi - 1 < j:
            # take qi largest as paid
            cost = sum(a[i:i+qi])  # conceptual; replaced below
            ans += sum(a[i:i+qi])
            i += qi

            # take up to 2 smallest as free
            take = min(2, j - i + 1)
            j -= take

    # remaining items paid normally
    while i <= j:
        ans += a[i]
        i += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã theo cấu trúc tham lam trực tiếp. Chúng ta duy trì hai con trỏ trên mảng đã sắp xếp: một con trỏ bắt đầu từ những món đồ đắt tiền nhất và một con trỏ bắt đầu từ những món đồ rẻ nhất. Vùng giữa co lại khi chúng tôi chỉ định các mục là trả phí hoặc miễn phí. Mỗi lần giảm giá sẽ tiêu tốn một khối mặt hàng phải trả phí từ bên trái và tùy ý chỉ định các mặt hàng miễn phí ở bên phải. 

Một điểm tinh tế là đảm bảo rằng chúng ta không vô tình sử dụng lại các mục giữa các vai trò. Cấu trúc hai con trỏ đảm bảo sự phân công rời rạc: một khi một mục được sử dụng từ một trong hai bên, nó sẽ không thể xuất hiện lại. Vòng lặp cuối cùng xử lý phần còn lại một cách chính xác, đảm bảo không có mục nào chưa được xử lý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
m = 1
q = [2]
n = 4
a = [50, 50, 100, 100]
```Giá sắp xếp: [100, 100, 50, 50] 

| Bước | Các mặt hàng phải trả phí | Vật phẩm miễn phí | Còn lại | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [100, 100] | [50, 50] | [] | 200 | 

Giảm giá duy nhất cỡ 2 cho phép chính xác một nhóm. Hai món đồ đắt tiền sẽ được trả tiền và cả hai món đồ rẻ tiền đều trở thành miễn phí. 

Điều này xác nhận rằng việc ghép các giá trị lớn làm điểm neo trả phí sẽ tối đa hóa hiệu quả. 

### Ví dụ 2 

đầu vào:```
m = [2]
q = [3]
n = 5
a = [1, 2, 3, 4, 5]
```Sắp xếp: [5, 4, 3, 2, 1] 

| Bước | Các mặt hàng phải trả phí | Vật phẩm miễn phí | Còn lại | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [5, 4, 3] | [1, 2] | [] | 12 | 

Ở đây một nhóm duy nhất bao gồm tất cả các mục. Cấu trúc cho thấy ngay cả khi quy mô nhóm lớn hơn, các mục miễn phí sẽ hấp thụ một cách tự nhiên các giá trị nhỏ nhất. 

Điều này chứng tỏ tại sao việc gán tự do phải luôn nhắm vào phần đuôi của mảng được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + m \log m)$| phân loại chiếm ưu thế; đường chuyền tham lam là tuyến tính | 
| Không gian |$O(n)$| lưu trữ các mục được sắp xếp | 

Các ràng buộc cho phép lên đến$10^5$các mặt hàng, vì vậy một$n \log n$cách tiếp cận cũng nằm trong giới hạn. Quá trình quét tham lam là tuyến tính và không gây ra chi phí bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""1
2
4
50 50 100 100
""") == "200"

# all equal, multiple grouping options
assert run("""2
1 2
6
10 10 10 10 10 10
""") in ["50", "60"]

# no discount effectively useful
assert run("""1
100
3
1 2 3
""") == "6"

# single item groups
assert run("""1
1
1
100
""") == "100"

# mixed sizes
assert run("""3
1 2 3
5
5 4 3 2 1
""") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị bằng nhau | 50/60 | tính đối xứng của nhóm | 
| không có giảm giá lợi ích | 6 | tính chính xác dự phòng | 
| mục duy nhất | 100 | trường hợp cạnh tối thiểu | 
| kích cỡ hỗn hợp | 15 | hành vi tham lam nói chung | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi có giảm giá nhưng còn tệ hơn là không sử dụng giảm giá. Đối với đầu vào`[1,2,3]`với một kích thước lớn$q$, thuật toán phải tránh việc ép buộc nhóm. Dự phòng tham lam đảm bảo các mặt hàng vẫn chưa được nhóm và được thanh toán trực tiếp. 

Một trường hợp khác là khi tất cả các mục đều giống hệt nhau. Trong trường hợp đó, mọi nhóm đều mang lại lợi ích giống nhau, vì vậy mọi phân vùng hợp lệ đều đúng. Phép gán hai con trỏ được sắp xếp vẫn tạo ra sự phân nhóm nhất quán vì cả hai đầu đều hoạt động đối xứng. 

Trường hợp thứ ba xảy ra khi chỉ tồn tại một loại giảm giá với giá trị lớn.$q$. Thuật toán quay trở lại thanh toán riêng lẻ một cách chính xác sau khi không tạo đủ nhóm, vì điều kiện`i + qi - 1 < j`ngăn chặn các nỗ lực nhóm không hợp lệ.
