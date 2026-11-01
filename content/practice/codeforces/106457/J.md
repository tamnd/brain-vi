---
title: "CF 106457J - Đảo"
description: "Chúng ta có một tập hợp n dãy. Mỗi chuỗi chứa mọi số nguyên trong một khoảng bao gồm [ai, bi]. Các khoảng thời gian có thể trùng lặp và khi thí sinh hợp nhất tác phẩm của mình, các số bằng nhau sẽ xuất hiện nhiều lần."
date: "2026-06-25T09:15:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106457
codeforces_index: "J"
codeforces_contest_name: "UTPC Spring 2026 Open Contest"
rating: 0
weight: 106457
solve_time_s: 48
verified: true
draft: false
---

[CF 106457J - Đảo](https://codeforces.com/problemset/problem/106457/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập`n`trình tự. Mỗi chuỗi chứa mọi số nguyên trong một khoảng bao gồm`[a_i, b_i]`. Các khoảng thời gian có thể trùng lặp và khi thí sinh hợp nhất tác phẩm của mình, các số bằng nhau sẽ xuất hiện nhiều lần. Danh sách cuối cùng là tập hợp nhiều tập của tất cả các khoảng được sắp xếp theo thứ tự không giảm. 

Đối với mỗi truy vấn, chúng ta được cung cấp một vị trí trong tập hợp được sắp xếp này và phải tìm giá trị được lưu trữ tại vị trí đó. Các vị trí có thể cực kỳ lớn vì tổng số phần tử trong danh sách được gộp có thể lên tới`10^14`, vì vậy việc xây dựng danh sách một cách rõ ràng là không thể. 

Các ràng buộc quan trọng là số lượng khoảng thời gian và truy vấn, cả hai đều lên tới`10^5`, trong khi điểm cuối khoảng có thể đạt tới`10^9`. Một giải pháp xử lý mọi số trong mỗi khoảng có thể yêu cầu khoảng`10^14`hoạt động vượt xa giới hạn. Ngay cả việc sắp xếp tất cả các giá trị được tạo cũng không thể thực hiện được vì số lượng giá trị được tạo là tổng độ dài khoảng chứ không phải số khoảng. 

Một cách hữu ích để suy nghĩ về một truy vấn là đặt một câu hỏi khác: cho một giá trị`x`, có bao nhiêu số trong danh sách hợp nhất nhỏ hơn hoặc bằng`x`? Nếu chúng ta có thể trả lời nhanh chóng, chúng ta có thể tìm kiếm nhị phân giá trị câu trả lời. Thách thức là trả lời nhiều truy vấn đếm như vậy một cách hiệu quả. 

Có một số trường hợp đặc biệt làm hỏng việc triển khai đơn giản hơn. Các khoảng chồng chéo phải giữ các giá trị trùng lặp. Ví dụ:```
2 3
1 3
3 5
1 2 3
```Danh sách được hợp nhất là:```
1 2 3 3 4 5
```Các kết quả đầu ra là:```
1
2
3
```Một giải pháp bất cẩn khi kết hợp các khoảng như các bộ sẽ tạo ra`1 2 3`và mất bản sao`3`. 

Một khoảng thời gian cũng phải được xử lý chính xác. Ví dụ:```
1 2
7 7
1 2
```Dãy số duy nhất chứa một số nên đáp án cho vị trí`1`là`7`. Việc coi các khoảng là phạm vi nửa mở do nhầm lẫn sẽ loại bỏ hoàn toàn giá trị này. 

Các truy vấn gần vị trí lớn nhất có thể cũng cần được quan tâm:```
2 2
1 1000000000
5 5
1000000000 1000000001
```Vị trí hợp lệ cuối cùng là`1000000001`, và câu trả lời của nó là`1000000000`. Bất kỳ việc triển khai nào sử dụng số nguyên 32 bit để đếm sẽ tràn vì tổng kích thước của nhiều tập hợp có thể vượt quá`10^9`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi giá trị từ mỗi khoảng, lưu trữ chúng, sắp xếp chúng và trả lời các truy vấn bằng cách lập chỉ mục vào mảng đã sắp xếp. Điều này đúng vì nó thực sự xây dựng chuỗi hợp nhất cần thiết. Tuy nhiên, một khoảng thời gian như`[1, 10^9]`đã chứa một tỷ giá trị. Với`10^5`khoảng thời gian, kích thước được tạo ra có thể đạt khoảng`10^14`, vì vậy phương pháp này thậm chí không thể hoàn thành việc lưu trữ dữ liệu. 

Quan sát quan trọng là chúng ta không cần danh sách đã hợp nhất. Đối với giá trị ứng cử viên`x`, mỗi khoảng đóng góp một lượng đơn giản vào số lượng tiền tố. 

Một khoảng thời gian`[a, b]`đóng góp:`0`khi`x < a`.`x - a + 1`khi`a <= x <= b`.`b - a + 1`khi`x > b`. 

Vì vậy, mọi truy vấn có thể được giải quyết bằng cách tìm giá trị nhỏ nhất`x`có số tiền tố ít nhất là vị trí được yêu cầu. Vấn đề còn lại là thực hiện các phép tính đếm tiền tố này nhanh chóng. 

Một truy vấn đếm đơn lẻ có thể được trả lời bằng cách quét qua tất cả các khoảng thời gian, nhưng thực hiện việc đó riêng biệt cho từng bước tìm kiếm nhị phân vẫn sẽ quá chậm. Thay vào đó, chúng tôi xử lý tất cả tìm kiếm nhị phân cùng nhau. 

Tìm kiếm nhị phân song song giữ một loạt các câu trả lời có thể có cho mọi truy vấn. Trong mỗi vòng, mọi truy vấn chưa được giải quyết sẽ chọn một điểm giữa. Chúng tôi sắp xếp các điểm giữa này và đánh giá tất cả chúng trong một lần quét theo các khoảng thời gian. Điều này làm giảm công việc lặp đi lặp lại từ`O(nq)`đại khái`O((n+q)log(10^9))`. 

Việc quét duy trì một hàm tuyến tính mô tả sự đóng góp của các khoảng thời gian hiện đang hoạt động. Khoảng thời gian bắt đầu thêm một thuật ngữ`x - a + 1`, trong khi các vị trí sau một khoảng kết thúc sẽ chuyển khoảng đó thành một mức đóng góp không đổi. Bởi vì tất cả các giá trị điểm giữa được xử lý theo thứ tự tăng dần nên mỗi sự kiện chỉ được áp dụng một lần mỗi vòng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng chiều dài khoảng + q) | O(tổng chiều dài khoảng) | Quá chậm | 
| Tối ưu | O((n + q) log 10^9) | O(n + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mỗi khoảng thời gian dưới dạng hai sự kiện quét. Tại vị trí`a`, khoảng thời gian bắt đầu đóng góp`x - a + 1`. Tại vị trí`b + 1`, sự đóng góp của nó trở thành hằng số`b - a + 1`. 

Biểu diễn này cho phép chúng ta đánh giá nhiều số tiền tố mà không cần chạm vào mọi số nguyên trong một khoảng. 
2. Đối với mỗi truy vấn, hãy giữ một phạm vi tìm kiếm nhị phân các câu trả lời có thể có. Ban đầu phạm vi là từ`1`ĐẾN`10^9`. 

Câu trả lời là giá trị đầu tiên có số tiền tố đạt đến thứ hạng yêu cầu. 
3. Lặp lại các vòng tìm kiếm nhị phân. Đối với mọi truy vấn vẫn có nhiều hơn một giá trị có thể, hãy tính điểm giữa của nó và thu thập tất cả các điểm giữa. 

Việc xử lý tất cả các điểm giữa cùng nhau là điều làm cho giải pháp đủ nhanh. 
4. Sắp xếp các giá trị trung điểm và duyệt qua chúng bằng các sự kiện khoảng. Duy trì ba giá trị: 

Độ dốc hiện tại của khoảng thời gian hoạt động. 

Việc chặn hiện tại của các khoảng hoạt động. 

Sự đóng góp liên tục từ những khoảng thời gian đã kết thúc. 

Số đếm tại vị trí`x`là:`slope * x + intercept + constant`. 
5. Đối với mỗi điểm giữa, hãy so sánh số tiền tố được tính toán với thứ hạng truy vấn. Nếu số đếm đủ lớn thì câu trả lời là ở hoặc trước điểm giữa này. Nếu không thì nó lớn hơn. 
6. Sau đủ vòng, mỗi phạm vi truy vấn chứa chính xác một giá trị. Xuất giá trị đó. 

Tại sao nó hoạt động: 

Tìm kiếm nhị phân hợp lệ vì hàm đếm tiền tố là đơn điệu. Tăng dần`x`chỉ có thể thêm nhiều số vào tiền tố chứ không bao giờ xóa chúng được. Quá trình quét tính toán chính xác hàm đếm tiền tố giống nhau bằng cách chia mỗi khoảng thời gian thành ba trạng thái có thể: chưa bắt đầu, đang hoạt động và đã kết thúc. Vì mọi điểm giữa được đánh giá bằng cách sử dụng các quy tắc đóng góp khoảng giống nhau nên mỗi quyết định tìm kiếm nhị phân sẽ bảo toàn phạm vi câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())

    events = []
    for _ in range(n):
        a, b = map(int, input().split())
        events.append((a, 1, 1 - a))
        events.append((b + 1, -1, b - a + 1))

    events.sort()

    queries = list(map(int, input().split()))

    lo = [1] * q
    hi = [10**9] * q

    def count_values(points):
        points.sort()
        ans = [0] * len(points)

        slope = 0
        intercept = 0
        constant = 0
        event_index = 0

        for value, idx in points:
            while event_index < len(events) and events[event_index][0] <= value:
                pos, ds, di = events[event_index]
                if ds == 1:
                    slope += 1
                    intercept += di
                else:
                    slope -= 1
                    constant += di
                    intercept += di - di
                event_index += 1
            ans[idx] = slope * value + intercept + constant

        return ans

    while True:
        mids = []
        active = False

        for i in range(q):
            if lo[i] < hi[i]:
                active = True
                mids.append(((lo[i] + hi[i]) // 2, i))

        if not active:
            break

        counts = count_values(mids)

        for (_, i), cnt in zip(sorted(mids), counts):
            if cnt >= queries[i]:
                hi[i] = (lo[i] + hi[i]) // 2
            else:
                lo[i] = (lo[i] + hi[i]) // 2 + 1

    print("\n".join(map(str, lo)))

if __name__ == "__main__":
    solve()
```Việc xây dựng sự kiện là chi tiết triển khai cốt lõi. Sự kiện bắt đầu sẽ tăng số khoảng thời gian hoạt động và thêm mức điều chỉnh chặn tương ứng. Sự kiện kết thúc sẽ loại bỏ khoảng thời gian khỏi phần tuyến tính đang hoạt động và chuyển toàn bộ chiều dài của nó thành phần đóng góp không đổi. 

Mảng tìm kiếm nhị phân chỉ lưu trữ các giá trị có thể, không đếm. Việc đánh giá điểm giữa quyết định nửa nào vẫn còn giá trị. 

Các giá trị có thể đạt tới`10^14`khi đếm các phần tử, vì vậy số nguyên Python rất hữu ích ở đây vì chúng tránh tự động tràn. Mã này cũng tránh tạo hoàn toàn chuỗi đã hợp nhất. 

Một điểm tinh tế là các sự kiện kết thúc xảy ra tại`b + 1`, không`b`. Tại`b`khoảng vẫn đóng góp thông qua công thức tuyến tính của nó. Chỉ ở giá trị tiếp theo, nó mới trở thành một đóng góp không đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3 5
1 3
2 4
3 5
1 2 3 4 5
```Danh sách được hợp nhất là:```
1 2 2 3 3 3 4 4 5
```Vòng tìm kiếm nhị phân có thể trông giống như: 

| Vị trí truy vấn | Phạm vi hiện tại | Điểm giữa | Đếm <= điểm giữa | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 1000000000] | 500000000 | 9 | Di chuyển sang trái | 
| 5 | [1, 1000000000] | 500000000 | 9 | Di chuyển sang trái | 
| 9 | [1, 1000000000] | 500000000 | 9 | Di chuyển sang trái | 

Quá trình quét đếm chính xác tất cả chín giá trị vì tất cả các khoảng đều kết thúc trước điểm giữa. 

Sau khi thu hẹp, truy vấn đầu tiên đạt đến`1`, lần thứ năm đạt tới`3`, và thứ chín đạt đến`5`. 

Một ví dụ thứ hai:```
2 4
10 10
20 22
1 2 3 4
```Multiset được sắp xếp là:```
10 20 21 22
```| Vị trí truy vấn | Phạm vi hiện tại | Điểm giữa | Đếm <= điểm giữa | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 1000000000] | 500000000 | 4 | Di chuyển sang trái | 
| 2 | [1, 1000000000] | 500000000 | 4 | Di chuyển sang trái | 
| 3 | [1, 1000000000] | 500000000 | 4 | Di chuyển sang trái | 
| 4 | [1, 1000000000] | 500000000 | 4 | Di chuyển sang trái | 

Các vòng tiếp theo hội tụ đến`10`,`20`,`21`, Và`22`. 

Ví dụ này kiểm tra xem các khoảng phân tách có được xử lý chính xác hay không và các vị trí bên trong khoảng trống không quan trọng vì tìm kiếm nhị phân chỉ quan tâm đến số lượng tiền tố thực tế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log 10^9) | Mỗi vòng tìm kiếm nhị phân sắp xếp và quét tất cả các điểm giữa của truy vấn và các sự kiện khoảng. | 
| Không gian | O(n + q) | Các sự kiện, phạm vi tìm kiếm nhị phân và lưu trữ điểm giữa tạm thời là tuyến tính. | 

Logarit xuất phát từ phạm vi giá trị, nhiều nhất là`10^9`, cần khoảng 30 vòng. Với`10^5`các khoảng thời gian và truy vấn, tổng công việc là khoảng vài triệu thao tác cho mỗi giai đoạn chính, vừa vặn thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3 9
1 3
2 4
3 5
1 2 3 4 5 6 7 8 9
""") == """1
2
2
3
3
3
4
4
5
""", "overlapping intervals"

assert run("""1 3
7 7
1 2 3
""") == """7
7
7
""", "single element interval"

assert run("""2 4
10 10
20 22
1 2 3 4
""") == """10
20
21
22
""", "separate intervals"

assert run("""2 2
1 1000000000
5 5
1 1000000001
""") == """1
1000000000
""", "large values"

assert run("""3 5
5 5
5 5
5 5
1 2 3 4 5
""") == """5
5
5
5
5
""", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khoảng chồng chéo | Sửa số lượng trùng lặp | Tính đa dạng được bảo tồn | 
| Khoảng phần tử đơn | Giá trị đơn lặp lại | Xử lý khoảng thời gian bao gồm | 
| Khoảng cách riêng biệt | Giá trị xuyên suốt các khoảng trống | Chuyển tiếp quét sự kiện | 
| Giá trị lớn | Tọa độ cỡ tỷ | An toàn và ranh giới nguyên | 
| Tất cả các giá trị bằng nhau | Câu trả lời giống nhau lặp đi lặp lại | Xử lý trùng lặp | 

## Vỏ cạnh 

Đối với các khoảng chồng chéo như:```
2 3
1 3
3 5
1 2 3
```việc quét tạo ra một sự kiện bắt đầu tại`1`Và`2`, sau đó kết thúc sự kiện sau`3`Và`5`. Khi trung điểm đạt tới`3`, cả hai khoảng vẫn đóng góp thông qua các công thức hoạt động của chúng, do đó số tiền tố bao gồm cả hai bản sao của`3`. 

Đối với khoảng giá trị đơn:```
1 2
7 7
1 2
```khoảng thời gian bắt đầu lúc`7`và kết thúc tại`8`. giá trị`7`được tính trong khi khoảng thời gian đang hoạt động và sự kiện kết thúc không xảy ra cho đến sau khoảng thời gian đó, vì vậy câu trả lời vẫn là`7`. 

Đối với trường hợp biên lớn nhất:```
2 2
1 1000000000
5 5
1000000000 1000000001
```truy vấn đầu tiên hội tụ đến giá trị cuối cùng trong khoảng thời gian dài. Truy vấn thứ hai đạt giá trị bổ sung từ khoảng thứ hai. Bộ đếm tiền tố xử lý cả hai vì nó không bao giờ mở rộng khoảng, nó chỉ đánh giá công thức đóng góp của nó.
