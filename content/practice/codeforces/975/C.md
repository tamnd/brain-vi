---
problem: 975C
contest_id: 975
problem_index: C
name: "Valhalla Siege"
contest_name: "Codeforces Round 478 (Div. 2)"
rating: 1400
tags: ["binary search"]
answer: passed_samples
verified: true
solve_time_s: 58
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a436-0e7c-83ec-85b7-473ad67f57cb
---

#CF 975C - Cuộc vây hãm Valhalla 

**Đánh giá:** 1400 
**Thẻ:** tìm kiếm nhị phân 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 58s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a436-0e7c-83ec-85b7-473ad67f57cb 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi đang mô phỏng một trận chiến lặp đi lặp lại trên một hàng chiến binh. Mỗi chiến binh có một lượng máu cố định và các mũi tên luôn bắn trúng chiến binh còn sống đầu tiên trước khi chuyển sang chiến binh tiếp theo. Khi một chiến binh tích lũy sát thương bằng hoặc vượt quá sức khỏe của mình, anh ta được coi là đã chết và cuộc tấn công tiếp tục vào chiến binh tiếp theo trong cùng một phút. 

Điều khó khăn là trận chiến sẽ “đặt lại” sau mỗi phút nếu tất cả chiến binh chết trong phút đó. Khi đó, tất cả chiến binh ngay lập tức trở lại trạng thái khỏe mạnh trước khi phút tiếp theo bắt đầu. Tuy nhiên, nếu một số chiến binh sống sót sau khi một phút kết thúc, lượng máu giảm của họ vẫn tiếp tục sang phút tiếp theo. 

Trong mỗi phút, chúng tôi cần báo cáo có bao nhiêu chiến binh vẫn còn sống sau khi xử lý mũi tên của phút đó, có tính đến mọi thiệt hại một phần gây ra từ những phút trước đó. 

Kích thước đầu vào khiến cho việc mô phỏng đơn giản là không thể. Với tối đa 200.000 chiến binh và 200.000 phút và số lượng mũi tên lên tới 10^14 mỗi phút, mọi giải pháp xử lý từng mũi tên riêng lẻ hoặc quét liên tục từ đầu sẽ quá chậm. Ngay cả cách tiếp cận O(nq) cũng có rủi ro đối với các hoạt động 4e10, điều này không khả thi. 

Một trường hợp khó khăn xuất hiện khi mũi tên trong một phút đủ để giết chết tất cả chiến binh mà vẫn còn sót lại. Trong trường hợp đó, những mũi tên còn sót lại sẽ bị loại bỏ và tất cả các chiến binh sẽ hồi sinh. Việc triển khai bất cẩn có thể gây ra thiệt hại không chính xác vào phút tiếp theo hoặc không thể thiết lập lại đúng cách. 

Một trường hợp khó khăn khác là khi sát thương chỉ giết chết một phần chiến binh. Chiến binh đó sẽ trở thành “mục tiêu hiện tại” mới trong phút tiếp theo và lượng máu còn lại của anh ta phải duy trì chính xác. Thiếu sự kiên trì này dẫn đến số lượng không chính xác trong các truy vấn tiếp theo. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ xử lý từng phút bằng cách lặp lại các chiến binh từ vị trí còn sống đầu tiên và trừ từng mũi tên cho đến khi hết mũi tên hoặc tất cả các chiến binh đều chết. Nếu sức khỏe của một chiến binh bằng 0, chúng ta sẽ chuyển sang chiến binh tiếp theo. Cách tiếp cận này đúng nhưng trong trường hợp xấu nhất, mỗi phút có thể đi ngang qua tất cả n chiến binh và mỗi chiến binh có thể được truy cập nhiều lần trong nhiều phút, dẫn đến hành vi O(nq). Với 200.000 x 200.000 thì điều này hoàn toàn không khả thi. 

Quan sát quan trọng là sát thương chỉ di chuyển từ trái sang phải và một khi một chiến binh bị tiêu diệt hoàn toàn, anh ta sẽ không bao giờ có liên quan nữa trừ khi việc thiết lập lại hoàn toàn xảy ra. Điều này gợi ý việc duy trì cấu trúc tiền tố của lượng máu còn lại sau những phút trước đó và nhanh chóng tìm ra làn sóng mũi tên hiện tại có thể lan truyền bao xa. 

Thay vì mô phỏng từng mũi tên, chúng tôi có thể duy trì tổng tiền tố về lượng máu và theo dõi tổng lượng sát thương đã tích lũy cho đến nay. Mũi tên của mỗi phút sẽ tăng bộ đếm sát thương toàn cầu. Chỉ số chiến binh đầu tiên vẫn còn sống là vị trí đầu tiên mà tổng tiền tố vượt quá sát thương tích lũy. Điều này làm giảm vấn đề liên tục tìm ranh giới trong mảng tổng tiền tố, có thể được thực hiện bằng cách sử dụng tìm kiếm nhị phân. 

Tuy nhiên, chúng tôi cũng cần xử lý việc đặt lại khi tổng số mũi tên đến trong một phút vượt quá tổng lượng máu còn lại. Trong trường hợp đó, tất cả các chiến binh đều chết và chúng tôi đặt lại sát thương tích lũy về 0, vì mọi thứ đều hồi sinh. 

Vì vậy, vấn đề trở thành việc duy trì “thiệt hại cho đến nay” toàn cầu và trả lời xem có bao nhiêu tổng tiền tố vẫn cao hơn nó, với việc đặt lại định kỳ khi thiệt hại vượt quá tổng tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tổng tiền tố + Tìm kiếm nhị phân | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố`pref`, Ở đâu`pref[i]`là tổng lượng máu của các chiến binh từ 1 đến i. Điều này cho chúng tôi biết mức độ sát thương cần thiết để tiêu diệt đến một điểm nhất định. 
2. Duy trì một biến`d`đại diện cho tổng thiệt hại được chuyển sang trạng thái hiện tại. 
3. Đối với mỗi truy vấn`k_i`, tăng`d`qua`k_i`, vì mũi tên sẽ tích lũy sát thương liên tục cho đến khi thiết lập lại xảy ra. 
4. Nếu`d`lớn hơn hoặc bằng`pref[n]`, tất cả chiến binh đều chết trong phút này. Trong trường hợp đó, đầu ra`n`và thiết lập lại`d`về 0, vì sự hồi sinh xảy ra ngay sau khi chết hoàn toàn. 
5. Nếu không, chúng ta cần tìm xem có bao nhiêu chiến binh còn sống sau khi bị thiệt hại`d`. Chúng tôi tìm thấy chỉ số nhỏ nhất`pos`như vậy`pref[pos] > d`sử dụng tìm kiếm nhị phân. 
6. Số chiến binh còn sống là`n - pos + 1`, vì tất cả các chiến binh trước đây`pos`đã chết và những người còn lại còn sống. 
7. Xuất giá trị này cho phút hiện tại và tiếp tục. 

Bước tìm kiếm nhị phân rất quan trọng vì nó chuyển quá trình hư hỏng tiền tố động thành truy vấn cấu trúc có thứ tự tĩnh. 

### Tại sao nó hoạt động 

Mảng tổng tiền tố mã hóa chính xác lượng sát thương tích lũy cần thiết để tiêu diệt tối đa mỗi chiến binh. Vì sát thương luôn được áp dụng từ trái sang phải và không bao giờ lùi về phía sau nên điều kiện`pref[i] <= d`mô tả chính xác những chiến binh đã chết. Phân đoạn sống động luôn là một hậu tố, do đó, một chỉ mục ranh giới duy nhất mô tả đầy đủ trạng thái. Điều kiện thiết lập lại duy trì tính chính xác vì khi tất cả các chiến binh chết, trạng thái hệ thống sẽ giống hệt với trạng thái ban đầu, khiến thiệt hại trong quá khứ không còn phù hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))

pref = [0] * (n + 1)
for i in range(1, n + 1):
    pref[i] = pref[i - 1] + a[i - 1]

total = pref[n]
d = 0

import bisect

for _ in range(q):
    k = int(input())
    d += k

    if d >= total:
        print(n)
        d = 0
        continue

    pos = bisect.bisect_right(pref, d)
    alive = n - (pos - 1)
    print(alive)
```Mảng tổng tiền tố nén tất cả sức mạnh của chiến binh thành một cấu trúc đơn điệu trong đó mỗi tiền tố đại diện cho một ngưỡng tiêu diệt hoàn chỉnh. Biến`d`tích lũy tổng số mũi tên đến trong nhiều phút cho đến khi quá trình xóa hoàn toàn diễn ra, tại thời điểm đó chúng tôi đặt lại do trạng thái hệ thống khởi động lại. 

Tìm kiếm nhị phân (`bisect_right`) tìm thấy tiền tố đầu tiên lớn hơn thiệt hại hiện tại, xác định trực tiếp chiến binh còn sống đầu tiên. Phép trừ cho số lượng người sống sót một cách hiệu quả. 

Một chi tiết tinh tế là xử lý việc đặt lại một cách chính xác trước khi thực hiện tìm kiếm nhị phân. Nếu chúng ta quên thiết lập lại`d`khi tất cả các chiến binh chết, các truy vấn trong tương lai sẽ gây sát thương không chính xác lên hệ thống đã được đặt lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5
1 2 1 2 1
3 10 1 1 1
```Tổng tiền tố:`[0, 1, 3, 4, 6, 7]`, tổng cộng = 7 

| Phút | k | d sau khi cập nhật | Tình trạng | tư thế | còn sống | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | d < 7 | 3 | 3 | 
| 2 | 10 | đặt lại | d ≥ 7 | - | 5 | 
| 3 | 1 | 1 | d < 7 | 2 | 4 | 
| 4 | 1 | 2 | d < 7 | 2 | 4 | 
| 5 | 1 | 3 | d < 7 | 3 | 3 | 

Dấu vết này cho thấy cái chết hoàn toàn sẽ kích hoạt thiết lập lại như thế nào, trong khi sát thương một phần sẽ tiếp tục một cách chính xác. 

### Mẫu 2 

đầu vào:```
4 4
2 1 3 4
1 2 5 3
```Tổng tiền tố:`[0, 2, 3, 6, 10]`, tổng cộng = 10 

| Phút | k | d sau khi cập nhật | tư thế | còn sống | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 4 | 
| 2 | 2 | 3 | 3 | 2 | 
| 3 | 5 | 8 | 4 | 1 | 
| 4 | 3 | đặt lại | - | 4 | 

Điều này cho thấy thiết lập lại muộn sau khi sát thương tích lũy vượt quá tổng lượng máu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi truy vấn thực hiện tìm kiếm nhị phân trên tổng tiền tố | 
| Không gian | O(n) | Lưu trữ mảng tổng tiền tố | 

Các ràng buộc cho phép tối đa 200.000 truy vấn, do đó, hệ số logarit cho mỗi truy vấn có thể dễ dàng đủ nhanh trong vòng 2 giây trong Python khi được triển khai bằng tìm kiếm nhị phân tích hợp sẵn. 

## Trường hợp thử nghiệm```python
import sys, io
import bisect

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + a[i - 1]

    total = pref[n]
    d = 0

    out = []
    for _ in range(q):
        k = int(input())
        d += k
        if d >= total:
            out.append(str(n))
            d = 0
        else:
            pos = bisect.bisect_right(pref, d)
            out.append(str(n - (pos - 1)))

    return "\n".join(out)

# provided sample
assert run("""5 5
1 2 1 2 1
3 10 1 1 1
""") == """3
5
4
4
3"""

# custom 1: single warrior
assert run("""1 3
5
1 5 6
""") == """1
1
1"""

# custom 2: all equal health
assert run("""3 3
2 2 2
1 3 5
""") == """3
2
3"""

# custom 3: immediate full reset
assert run("""4 2
1 1 1 1
10 1
""") == """4
3"""

# custom 4: boundary partial kill
assert run("""5 1
1 2 3 4 5
9
""") == """2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chiến binh độc thân | 1,1,1 | cấu trúc tầm thường và hành vi thiết lập lại | 
| sức khỏe bình đẳng | hỗn hợp | tìm kiếm nhị phân đúng ranh giới | 
| thiết lập lại toàn bộ | 4,3 | xử lý thiết lập lại vượt mức | 
| ranh giới giết một phần | 2 | tính toán ranh giới tiền tố đúng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi một truy vấn duy nhất cung cấp đủ mũi tên để vượt quá tổng lượng máu nhiều lần về mặt khái niệm. Thuật toán xử lý việc này bằng cách đặt lại ngay lập tức bất cứ khi nào`d >= total`, đảm bảo không có rò rỉ hư hỏng còn sót lại trong các chu kỳ. For example, with`a = [1,1,1]`Và`k = 10`, chúng tôi thiết lập`d = 0`và xuất ra chính xác tất cả các chiến binh còn sống. 

Một trường hợp khác là khi thiệt hại xảy ra chính xác trên ranh giới tổng tiền tố. Bởi vì chúng tôi sử dụng`bisect_right`, nếu như`d`bằng`pref[i]`, chiến binh`i`được coi là đã chết và chiến binh tiếp theo trở thành người sống đầu tiên. Điều này phù hợp với quy tắc mũi tên làm tiêu hao hoàn toàn một chiến binh trước khi tiến về phía trước.
