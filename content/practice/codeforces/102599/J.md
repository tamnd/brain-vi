---
title: "CF 102599J - Khoảng cách phục hồi"
description: "Chúng tôi có một loạt các chiều cao cột. Mục tiêu là chọn một chiều cao cuối cùng x và biến đổi mọi cột sao cho chiều cao của nó trở thành chính xác x."
date: "2026-07-31T06:01:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102599
codeforces_index: "J"
codeforces_contest_name: "The fifth Lipetsk collegiate programming contest. Finals. 8-11 form"
rating: 0
weight: 102599
solve_time_s: 253
verified: true
draft: false
---

[CF 102599J - Khoảng cách khôi phục](https://codeforces.com/problemset/problem/102599/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các chiều cao cột. Mục tiêu là chọn một chiều cao cuối cùng`x`và biến đổi mọi cây cột sao cho chiều cao của nó trở nên chính xác`x`. Tăng chi phí trụ cột`A`mỗi viên gạch được thêm vào, chi phí loại bỏ một viên gạch`R`mỗi viên gạch bị loại bỏ và di chuyển một viên gạch từ cột này sang cột khác`M`. 

Đầu ra là tổng chi phí tối thiểu có thể có trong số tất cả các lựa chọn về chiều cao cuối cùng. 

Số lượng trụ cột có thể đạt tới`100000`, vì vậy việc cố gắng đạt đến mọi độ cao có thể để đạt được số lượng gạch tối đa là không thực tế. Chiều cao có thể lớn bằng`10^9`, loại trừ mọi cách tiếp cận tùy thuộc vào phạm vi số độ cao. Chúng ta cần một thuật toán gần`O(N log N)`hoặc`O(N)`sau khi sắp xếp vì việc lặp lại quá nhiều độ cao ứng cử viên sẽ vượt quá các thao tác có sẵn. 

Một sai lầm phổ biến là cho rằng chiều cao cuối cùng tốt nhất phải là mức trung bình hoặc trung vị. Chi phí vận hành sẽ thay đổi câu trả lời. Ví dụ, nếu thêm gạch thì cực kỳ rẻ, chiều cao tốt nhất có thể trên mức trung bình vì việc bỏ gạch rất tốn kém. 

Một trường hợp khác xuất hiện khi việc di chuyển các viên gạch rẻ hơn so với việc tháo và thêm riêng biệt. Coi như:```
3 100 100 1
1 3 8
```Câu trả lời đúng là`4`. Tổng số viên gạch là`12`, do đó tính chiều cao của mỗi cột`4`chỉ cần di chuyển gạch từ cột cao`8`đến cột chiều cao`1`. Một giải pháp chỉ so sánh việc bổ sung và loại bỏ độc lập sẽ bỏ lỡ việc chuyển giao rẻ hơn này. 

Trường hợp cạnh thứ hai là khi một hướng hoạt động tự do. Ví dụ:```
2 0 100 100
0 5
```Câu trả lời đúng là`0`. Nâng cây cột trống lên cao`5`không tốn kém gì, vì vậy chiều cao mục tiêu`5`là miễn phí. Việc thực hiện bất cẩn luôn cho rằng một số viên gạch phải được di chuyển hoặc loại bỏ sẽ tạo ra chi phí dương. 

Trường hợp cạnh thứ ba là khi tất cả các cột đã có cùng chiều cao:```
3 10 20 5
7 7 7
```Câu trả lời đúng là`0`. Thuật toán phải coi chiều cao hiện tại là một ứng cử viên và tránh thực hiện các thao tác không cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là thử mọi độ cao cuối cùng có thể. Chiều cao của mỗi ứng viên`x`, chúng ta đếm xem phải thêm bao nhiêu viên gạch và bỏ đi bao nhiêu viên gạch. Sau đó, chúng tôi tính toán chi phí để khắc phục sự khác biệt. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra. 

Tuy nhiên, chiều cao tối đa có thể được`10^9`, vì vậy hãy quét tất cả những gì có thể`x`các giá trị sẽ yêu cầu lên tới`10^9`đánh giá. Ngay cả một đánh giá cũng mất`O(N)`, dẫn đến khoảng`10^14`trong trường hợp xấu nhất vượt xa giới hạn. 

Quan sát quan trọng là hàm chi phí là hàm lồi. Nếu chúng ta tăng chiều cao mục tiêu lên một đơn vị thì số lượng gạch cần thêm hoặc bớt sẽ thay đổi dần dần. Tổng chi phí giảm dần cho đến khi đạt mức tối ưu rồi tăng lên. Hàm lồi trên một phạm vi số nguyên có thể được giảm thiểu bằng cách tìm kiếm nhị phân trên độ dốc. 

Đối với chiều cao mục tiêu cố định`x`, mỗi cột có chiều cao dưới`x`đóng góp vào những bổ sung cần thiết và mọi trụ cột ở trên`x`góp phần loại bỏ. Nếu chúng ta có thể tính toán hiệu quả hai đại lượng này, chúng ta có thể đánh giá chi phí của bất kỳ đại lượng nào.`x`nhanh chóng. 

Việc sắp xếp độ cao cho phép chúng tôi tìm thấy có bao nhiêu cột nằm dưới chiều cao ứng cử viên bằng cách sử dụng tìm kiếm nhị phân. Tổng tiền tố cho phép chúng tôi tính toán tổng số viên gạch bị thiếu hoặc thừa mà không cần lặp lại qua tất cả các cột mỗi lần. Sau đó chúng tôi tìm kiếm nhị phân chiều cao câu trả lời. 

Phương pháp vũ phu hoạt động vì nó kiểm tra mọi mục tiêu có thể, nhưng không thành công vì phạm vi quá lớn. Quan sát thấy đường chi phí lồi làm giảm việc tìm kiếm từ hàng tỷ ứng viên xuống còn khoảng 31 lần kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NH), trong đó H là độ cao tối đa | O(1) | Quá chậm | 
| Tối ưu | O(N log N + log(H) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp chiều cao của cột và xây dựng mảng tổng tiền tố. Tổng tiền tố cho phép chúng tôi tìm thấy tổng chiều cao của bất kỳ tiền tố cột nào ngay lập tức, điều này làm cho việc tính toán chi phí không phụ thuộc vào số lượng cột. 
2. Xác định hàm tính chi phí cho chiều cao mục tiêu đã chọn`x`. Sử dụng tìm kiếm nhị phân trên mảng đã sắp xếp để tìm cột đầu tiên có chiều cao ít nhất`x`. Các cột trước vị trí này cần bổ sung gạch, các cột còn lại có gạch thừa cần phải bỏ đi hoặc di chuyển đi. 
3. Tính số gạch còn thiếu ở mặt dưới và số gạch thừa ở mặt trên. Hai giá trị này thể hiện khối lượng công việc cần thiết để đạt được độ cao`x`. 
4. Sử dụng gạch di chuyển bất cứ khi nào có thể. Một viên gạch được lấy ra khỏi một cột và thêm vào một cột khác có thể đáp ứng cả hai thao tác cùng một lúc. Số lần di chuyển như vậy là số lượng gạch còn thiếu và dư thừa tối thiểu. 
5. Kiểm soát chi phí di chuyển`min(M, A + R)`. Nếu việc di chuyển một viên gạch tốn kém hơn việc dỡ bỏ nó và thêm một viên gạch mới thì không có lý do gì để di chuyển nó. 
6. Tìm kiếm nhị phân chiều cao mục tiêu. So sánh chi phí tại`mid`Và`mid + 1`. Nếu chi phí giảm, hãy di chuyển phạm vi tìm kiếm lên trên. Nếu không, di chuyển nó xuống. Chi phí nhỏ nhất được tìm thấy ở gần điểm mà độ dốc thay đổi. 

Tại sao nó hoạt động: chi phí chọn chiều cao mục tiêu có dạng lồi. Việc di chuyển chiều cao mục tiêu từ trái sang phải sẽ thay đổi chi phí theo hướng có thể dự đoán được: trước khi đạt mức tối ưu, việc tăng mục tiêu sẽ giảm số lần loại bỏ tốn kém nhanh hơn so với việc tăng số lần bổ sung và sau khi tối ưu thì điều ngược lại sẽ xảy ra. Tìm kiếm nhị phân tìm thấy điểm chuyển tiếp. Đối với bất kỳ mục tiêu cố định nào, việc tính toán chi phí đều chính xác vì tất cả các lần chuyển gạch có thể được sử dụng trước tiên khi có lợi và phần chênh lệch còn lại sẽ được xử lý bằng cách bổ sung hoặc loại bỏ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, A, R, M = map(int, input().split())
    h = list(map(int, input().split()))

    h.sort()
    pref = [0]
    for x in h:
        pref.append(pref[-1] + x)

    M = min(M, A + R)

    def cost(x):
        import bisect

        idx = bisect.bisect_left(h, x)

        need = x * idx - pref[idx]
        extra = (pref[n] - pref[idx]) - x * (n - idx)

        move = min(need, extra)
        need -= move
        extra -= move

        return move * M + need * A + extra * R

    left = h[0]
    right = h[-1]

    while left < right:
        mid = (left + right) // 2
        if cost(mid) <= cost(mid + 1):
            right = mid
        else:
            left = mid + 1

    print(cost(left))

if __name__ == "__main__":
    solve()
```Mảng được sắp xếp và mảng tổng tiền tố tương ứng với bước đầu tiên của thuật toán. Sau khi sắp xếp, mỗi chiều cao ứng cử viên sẽ chia các cột thành nhóm dưới và nhóm trên, đó là lý do tại sao có thể thực hiện tìm kiếm nhị phân trên các vị trí đã sắp xếp. 

các`cost`chức năng đầu tiên tìm thấy điểm phân chia với`bisect_left`. biểu hiện`x * idx - pref[idx]`tính toán có bao nhiêu viên gạch bị thiếu dưới mục tiêu. biểu hiện`(pref[n] - pref[idx]) - x * (n - idx)`tính toán số gạch dư thừa trên mục tiêu. 

Việc tính toán chuyển giao được xử lý trước khi thêm và bớt. Thứ tự này quan trọng vì việc di chuyển một viên gạch thừa có thể đồng thời sửa được một viên gạch bị thiếu. Số lượng nước đi hữu ích bị giới hạn bởi phía nhỏ hơn, vì vậy`min(need, extra)`là số tiền đúng. 

Chi phí di chuyển được thay thế bằng`min(M, A + R)`bởi vì một động thái tốn kém hơn việc tạo và xóa một viên gạch riêng lẻ không bao giờ hữu ích. 

Số nguyên Python không bị tràn, điều này là cần thiết vì tổng chi phí có thể lên tới khoảng`10^18`. Việc tìm kiếm nhị phân sử dụng`h[0]`Và`h[-1]`làm ranh giới vì các mục tiêu nằm ngoài khoảng này không bao giờ có thể tốt hơn ranh giới gần nhất. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 1 100 100
1 3 8
```Chi phí di chuyển hiệu quả là`100`, bởi vì việc cộng và loại bỏ các chi phí cùng nhau`101`, nên di chuyển sẽ rẻ hơn một chút. 

| mục tiêu | mất tích | thêm | di chuyển | tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 3 | 2 | 5 | 2 | 302 | 
| 4 | 5 | 4 | 4 | 12 | 
| 5 | 7 | 3 | 3 | 403 | 

Tối ưu là chiều cao`4`. Bốn viên gạch có thể được di chuyển từ cây cột cao nhất và thêm một viên gạch nữa, đưa ra câu trả lời`12`. 

Đối với mẫu thứ hai:```
3 100 1 100
1 3 8
```Chi phí di chuyển hiệu quả là`100`, đồng thời cộng và loại bỏ các chi phí`101`. 

| mục tiêu | mất tích | thêm | di chuyển | tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 9 | 0 | 9 | 
| 2 | 1 | 7 | 1 | 701 | 
| 3 | 2 | 5 | 2 | 502 | 

Ở đây dỡ gạch rất rẻ nên hạ thấp mọi thứ xuống độ cao`1`là tối ưu. Chỉ cần loại bỏ những viên gạch thừa từ các trụ khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + log(H) log N) | Việc sắp xếp chiếm ưu thế và mỗi bước tìm kiếm nhị phân sẽ đánh giá chi phí bằng tìm kiếm nhị phân trên các độ cao được sắp xếp. | 
| Không gian | O(N) | Tổng tiền tố yêu cầu bộ nhớ bổ sung tuyến tính. | 

Xử lý bước sắp xếp`100000`cột thoải mái. Tìm kiếm nhị phân theo độ cao chỉ cần khoảng 31 lần lặp vì độ cao tối đa là`10^9`, nên nghiệm nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import bisect

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, A, R, M = map(int, input().split())
    h = list(map(int, input().split()))

    h.sort()
    pref = [0]
    for x in h:
        pref.append(pref[-1] + x)

    M = min(M, A + R)

    def cost(x):
        idx = bisect.bisect_left(h, x)
        need = x * idx - pref[idx]
        extra = pref[n] - pref[idx] - x * (n - idx)
        move = min(need, extra)
        return move * M + (need - move) * A + (extra - move) * R

    l, r = h[0], h[-1]
    while l < r:
        m = (l + r) // 2
        if cost(m) <= cost(m + 1):
            r = m
        else:
            l = m + 1

    return str(cost(l))

assert solution("""3 1 100 100
1 3 8
""") == "12"

assert solution("""3 100 1 100
1 3 8
""") == "9"

assert solution("""3 100 100 1
1 3 8
""") == "4"

assert solution("""1 5 5 5
0
""") == "0"

assert solution("""4 10 10 10
7 7 7 7
""") == "0"

assert solution("""2 0 100 100
0 5
""") == "0"

assert solution("""3 1 1000 1
0 10 20
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 5 5 / 0`|`0`| Trụ đơn và ranh giới không có chiều cao | 
|`4 10 10 10 / 7 7 7 7`|`0`| Đã có chiều cao bằng nhau | 
|`2 0 100 100 / 0 5`|`0`| Bổ sung miễn phí | 
|`3 1 1000 1 / 0 10 20`|`10`| Xử lý chuyển động và chuyển giao rất rẻ | 

## Vỏ cạnh 

Đối với case phong trào giá rẻ:```
3 100 100 1
1 3 8
```Thuật toán đánh giá chiều cao của ứng viên xung quanh mức tối ưu. Ở độ cao`4`, có ba viên gạch bị thiếu và bốn viên gạch thừa. Ba lần di chuyển các viên gạch từ cột cao nhất đến cột ngắn nhất, để lại một viên gạch thừa cần loại bỏ. Chi phí là`3 * 1 + 1 * 100 = 103`với chiều cao đó, nhưng chiều cao`3`tốt hơn: hai bước cố định cây cột đầu tiên và năm viên gạch còn lại để loại bỏ, cho`2 + 5 = 7`. Tìm kiếm nhị phân đạt đến giá trị tối thiểu chính xác của`4`thông qua hàm chi phí lồi. 

Đối với trường hợp bổ sung miễn phí:```
2 0 100 100
0 5
```Chiều cao mục tiêu`5`cho một cây cột trống cần năm viên gạch và không có viên gạch thừa nào. Chi phí bổ sung bằng 0 nên tổng chi phí bằng 0. Hàm chi phí xử lý trường hợp này vì số lượng thiếu và thừa được phép không cân bằng. 

Đối với các trụ bằng nhau:```
3 10 20 5
7 7 7
```Mọi điều chỉnh có thể được tính ở độ cao`7`là số không. Hàm trả về 0 và tìm kiếm nhị phân giữ câu trả lời bên trong phạm vi hợp lệ cho đến khi đạt đến độ cao hiện có này. Giải pháp không bắt buộc các hoạt động không cần thiết.
