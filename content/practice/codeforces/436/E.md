---
title: "CF 436E - Hộp Bìa Cứng"
description: "Mỗi cấp độ có thể kết thúc ở một trong ba trạng thái. Trạng thái 0 có nghĩa là chúng tôi bỏ qua nó và không đạt được sao nào. Trạng thái 1 có nghĩa là chúng tôi hoàn thành nó với giá một sao và dành [i] thời gian. Trạng thái 2 có nghĩa là chúng tôi hoàn thành nó với giá hai sao và dành b[i] thời gian."
date: "2026-06-07T02:55:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 436
codeforces_index: "E"
codeforces_contest_name: "Zepto Code Rush 2014"
rating: 2600
weight: 436
solve_time_s: 124
verified: false
draft: false
---

[CF 436E - Hộp bìa cứng](https://codeforces.com/problemset/problem/436/E) 

**Đánh giá:** 2600 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cấp độ có thể kết thúc ở một trong ba trạng thái. 

Tình trạng`0`có nghĩa là chúng ta bỏ qua nó và không đạt được ngôi sao nào. 

Tình trạng`1`có nghĩa là chúng tôi hoàn thành nó với giá một sao và chi tiêu`a[i]`thời gian. 

Tình trạng`2`có nghĩa là chúng tôi hoàn thành nó với giá hai sao và chi tiêu`b[i]`thời gian. 

Một cấp độ chỉ có thể chơi một lần, vì vậy việc chọn hai sao không **không** có nghĩa là phải trả tiền`a[i] + (b[i]-a[i])`. Tổng chi phí chỉ đơn giản là`b[i]`. 

Chúng ta cần sự phân công cuối cùng các trạng thái có tổng số sao ít nhất là`w`, đồng thời giảm thiểu tổng thời gian. Chúng ta cũng phải đưa ra một bài tập tối ưu. 

Những ràng buộc là điều làm cho vấn đề trở nên thú vị. Có tới`3·10^5`cấp độ, do đó, bất kỳ chương trình động nào về số lượng sao và cấp độ là hoàn toàn không thể. Thậm chí một`O(n√n)`giải pháp sẽ không thoải mái. Chúng ta cần một cái gì đó gần gũi`O(n log n)`. 

Một cái bẫy tinh vi đang xử lý từng ngôi sao một cách độc lập. Coi như:```
2 2
3 4
2 5
```Lấy ngôi sao rẻ nhất trước sẽ mang lại cho cấp độ đầu tiên một sao về chi phí`2`, sau đó là một ngôi sao khác cho chi phí`3`, tổng cộng`5`. Nhưng việc hoàn thành cấp độ đầu tiên trực tiếp chỉ tốn hai sao`4`. Bất kỳ kẻ tham lam nào chỉ nhìn vào ngôi sao riêng lẻ tiếp theo đều có thể thất bại. 

Một cái bẫy khác là giả định rằng khi một cấp độ nhận được một sao thì cấp độ đó phải duy trì ít nhất một sao trong mọi giải pháp tối ưu. Đôi khi giải pháp tốt nhất thay thế cấp một sao bằng cấp hai sao hoàn toàn khác. Thuật toán tối ưu cần có cách "hoàn tác" các quyết định trước đó. 

Ví dụ:```
3 2
1 100
2 3
2 3
```Một quy trình ngây thơ trước tiên có thể đạt được mức độ chi phí`1`cho một ngôi sao. Sau này sẽ tốt hơn nếu loại bỏ sự lựa chọn đó và chọn một trong những lựa chọn`b=3`cấp độ cho hai ngôi sao. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Mỗi cấp độ có ba trạng thái có thể có, vì vậy có`3^n`bài tập. Chúng ta có thể tính toán tổng số sao và tổng thời gian cho mỗi bài tập và giữ lại bài tập hợp lệ nhất. Điều này rõ ràng là đúng vì nó kiểm tra mọi thứ. 

Không may thay,`3^300000`vượt ra ngoài thiên văn. 

Nhận xét quan trọng là chúng ta không thực sự chọn những bài tập hoàn chỉnh. Chúng tôi đang tăng dần tổng số sao kiếm được từ`0`lên đến`w`. 

Giả sử chúng ta đã có một số cấu hình tối ưu với chính xác`k`các ngôi sao. Để có được`k+1`sao, có thể có một số sửa đổi khác nhau. 

Chúng tôi có thể tặng một sao cho mức độ hiện chưa được sử dụng. 

Chúng ta có thể nâng cấp cấp một sao lên cấp hai sao. 

Chúng ta thậm chí có thể thực hiện thao tác "hối tiếc": loại bỏ các ngôi sao khỏi một cấp độ và bù lại bằng cách biến một cấp độ khác thành cấp độ hai sao. Một động thái như vậy sẽ thay đổi nhiệm vụ trong khi vẫn tăng tổng số sao lên đúng một. 

Quan điểm này biến vấn đề thành việc mua đi mua lại theo cách rẻ nhất để tăng số lượng sao lên một. Khó khăn là hành động rẻ nhất có thể liên quan đến việc hủy bỏ các lựa chọn trước đó. Đó là lý do tại sao sự tham lam thông thường lại thất bại và sự tham lam hối hận lại thành công. 

Giải pháp thu được duy trì năm hàng ưu tiên thể hiện tất cả các cách có thể để đạt được thêm một sao. Mỗi lần lặp lại sẽ chọn tùy chọn có sẵn rẻ nhất và cập nhật trạng thái cấp độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| Hối Tiếc Tham Lam + Đống | O(w log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`star[i]`là trạng thái hiện tại của cấp độ`i`, ban đầu tất cả đều bằng không. 

Chúng tôi duy trì đống năm phút.`q1`lưu trữ các mức chưa sử dụng bằng cách`a[i]`.`q2`lưu trữ cấp độ một sao bằng cách`b[i]-a[i]`, chi phí bổ sung cần thiết để nâng cấp chúng lên hai sao.`q3`lưu trữ các mức chưa sử dụng bằng cách`b[i]`.`q4`lưu trữ cấp độ một sao bằng cách`-a[i]`.`q5`lưu trữ cấp độ hai sao bằng cách`-(b[i]-a[i])`. 

Hai đống cuối cùng trông có vẻ kỳ lạ nhưng chúng cho phép thực hiện các phép toán hối tiếc. 

Đối với mỗi`w`những ngôi sao chúng ta cần: 

1. Xóa các mục heap lỗi thời có trạng thái được lưu trữ không còn khớp với trạng thái hiện tại của cấp đó. 
2. Tính chi phí của bốn cách có thể để đạt được đúng một ngôi sao.`x1 = min a[i]`giữa các mức chưa sử dụng. 

Điều này có nghĩa là biến một`0-star`cấp độ vào một`1-star`mức độ. 
3. Tính toán`x2 = min (b[i]-a[i])`giữa các cấp độ một sao. 

Điều này có nghĩa là nâng cấp một`1-star`cấp độ vào một`2-star`mức độ. 
4. Tính toán`x3 = min b[j] - max a[i]`. 

Sử dụng đống, đây là`q3.top + q4.top`. 

Ở đây, chúng tôi xóa cấp một sao đã chọn trước đó và thay vào đó tạo một số cấp chưa sử dụng có giá trị hai sao. Mức tăng ròng về số sao vẫn chính xác là một. 
5. Tính toán`x4 = min b[j] - max (b[i]-a[i])`. 

Sử dụng đống, đây là`q3.top + q5.top`. 

Ở đây, chúng tôi hạ cấp hai sao xuống một sao và biến một cấp độ không sử dụng khác thành hai sao. 
6. Chọn cái nhỏ nhất trong số đó`x1`,`x2`,`x3`, Và`x4`. 
7. Áp dụng chuyển đổi trạng thái tương ứng và cập nhật các vùng dữ liệu có liên quan. 
8. Thêm chi phí đã chọn vào câu trả lời. 

Sau đó`w`lặp đi lặp lại, bài tập hiện tại chứa chính xác`w`sao và có tổng thời gian tối thiểu. 

### Tại sao nó hoạt động 

Sau khi kiếm được`k`sao, nhiệm vụ hiện tại là nhiệm vụ có chi phí tối thiểu trong số tất cả các nhiệm vụ với chính xác`k`các ngôi sao. 

Bốn sự chuyển đổi trên liệt kê mọi cách có thể để chuyển từ một`k`-gán sao cho một`(k+1)`-phân công sao đồng thời cho phép xem xét lại các cấp độ đã chọn trước đó. Sự chuyển đổi hối tiếc là phần quan trọng. Họ đảm bảo rằng nếu một quyết định cục bộ trước đó trở nên dưới mức tối ưu thì thuật toán có thể thay thế nó bằng một cấu hình toàn cục tốt hơn. 

Ở mỗi bước, chúng tôi chọn mức tăng một sao rẻ nhất có thể. Vì mọi`(k+1)`-sao phân công có thể được lấy từ một số`k`-gán sao thông qua một trong các loại chuyển đổi này, việc chọn chuyển tiếp tối thiểu sẽ duy trì tính tối ưu. Bằng quy nạp, phép gán sau`w`lần lặp là tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, w = map(int, input().split())

    a = [0] * n
    b = [0] * n

    for i in range(n):
        a[i], b[i] = map(int, input().split())

    star = [0] * n

    INF = 10**30

    q1 = [(INF, n)]
    q2 = [(INF, n)]
    q3 = [(INF, n)]
    q4 = [(INF, n)]
    q5 = [(INF, n)]

    for i in range(n):
        heapq.heappush(q1, (a[i], i))
        heapq.heappush(q3, (b[i], i))

    star.append(-1)  # sentinel

    def clean(heap, need_state):
        while len(heap) > 1 and star[heap[0][1]] != need_state:
            heapq.heappop(heap)

    ans = 0

    for _ in range(w):
        clean(q1, 0)
        clean(q2, 1)
        clean(q3, 0)
        clean(q4, 1)
        clean(q5, 2)

        x1 = q1[0][0]
        x2 = q2[0][0]
        x3 = q3[0][0] + q4[0][0]
        x4 = q3[0][0] + q5[0][0]

        best = min(x1, x2, x3, x4)
        ans += best

        if best == x1:
            _, i = heapq.heappop(q1)

            star[i] = 1

            heapq.heappush(q2, (b[i] - a[i], i))
            heapq.heappush(q4, (-a[i], i))

        elif best == x2:
            _, i = heapq.heappop(q2)

            star[i] = 2

            heapq.heappush(q5, (-(b[i] - a[i]), i))

        elif best == x3:
            _, j = heapq.heappop(q3)
            _, i = heapq.heappop(q4)

            star[j] = 2
            star[i] = 0

            heapq.heappush(q5, (-(b[j] - a[j]), j))

            heapq.heappush(q1, (a[i], i))
            heapq.heappush(q3, (b[i], i))

        else:
            _, j = heapq.heappop(q3)
            _, i = heapq.heappop(q5)

            star[j] = 2
            star[i] = 1

            heapq.heappush(q5, (-(b[j] - a[j]), j))

            heapq.heappush(q2, (b[i] - a[i], i))
            heapq.heappush(q4, (-a[i], i))

    print(ans)
    print("".join(str(star[i]) for i in range(n)))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo bốn loại chuyển đổi trực tiếp. 

Việc xóa lười biếng là điều cần thiết. Một cấp độ có thể xuất hiện nhiều lần trong một đống vì trạng thái của nó thay đổi trong các hoạt động hối tiếc. Thay vì loại bỏ các mục cũ ngay lập tức, chúng tôi để chúng trong đống và loại bỏ chúng khi chúng đạt đến đỉnh. 

Những đống`q4`Và`q5`lưu trữ các giá trị âm. Một đống tối thiểu trên`-a[i]`cung cấp quyền truy cập vào lớn nhất`a[i]`, đó chính xác là những gì công thức hối tiếc cần. Ý tưởng tương tự áp dụng cho`-(b[i]-a[i])`. 

Tất cả số học đều sử dụng số nguyên Python để xử lý câu trả lời tối đa có thể một cách an toàn. Với tối đa`3·10^5`mức độ và chi phí lên tới`10^9`, tổng câu trả lời có thể vượt quá phạm vi 32 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
1 2
1 2
```| Bước | Di chuyển tốt nhất | Chi phí bổ sung | Kỳ | 
| --- | --- | --- | --- | 
| 1 | 0 → 1 ở cấp 1 | 1 | 10 | 
| 2 | 0 → 1 ở cấp 2 | 1 | 11 | 
| 3 | 1 → 2 ở cấp độ 1 | 1 | 21 | 

Tổng chi phí =`3`. 

Nhiệm vụ cuối cùng`21`kiếm được`3`sao có tổng thời gian tối thiểu. 

### Mẫu 2 

đầu vào:```
5 3
10 20
5 10
10 20
6 9
25 30
```| Bước | Di chuyển tốt nhất | Chi phí bổ sung | Kỳ | 
| --- | --- | --- | --- | 
| 1 | Cấp 2: 0 → 1 | 5 | 01000 | 
| 2 | Cấp 4: 0 → 2 | 9 | 01020 | 
| 3 | Đã có 3 sao | - | 01020 | 

Tổng chi phí =`14`. 

Ví dụ này cho thấy tại sao việc trực tiếp nhận cấp độ hai sao có thể tốt hơn việc thu thập hai lần hoàn thành một sao riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(w log n) | Mỗi lần lặp thực hiện một số lượng thao tác heap không đổi | 
| Không gian | O(n) | Năm đống và mảng trạng thái lưu trữ các mục O(n) | 

Từ`w ≤ 2n`, thời gian chạy là`O(n log n)`. Với`n = 3·10^5`, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    import heapq

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    input = sys.stdin.readline

    n, w = map(int, input().split())

    a = [0] * n
    b = [0] * n

    for i in range(n):
        a[i], b[i] = map(int, input().split())

    star = [0] * n

    INF = 10**30

    q1 = [(INF, n)]
    q2 = [(INF, n)]
    q3 = [(INF, n)]
    q4 = [(INF, n)]
    q5 = [(INF, n)]

    for i in range(n):
        heapq.heappush(q1, (a[i], i))
        heapq.heappush(q3, (b[i], i))

    star.append(-1)

    def clean(heap, need):
        while len(heap) > 1 and star[heap[0][1]] != need:
            heapq.heappop(heap)

    ans = 0

    for _ in range(w):
        clean(q1, 0)
        clean(q2, 1)
        clean(q3, 0)
        clean(q4, 1)
        clean(q5, 2)

        x1 = q1[0][0]
        x2 = q2[0][0]
        x3 = q3[0][0] + q4[0][0]
        x4 = q3[0][0] + q5[0][0]

        best = min(x1, x2, x3, x4)
        ans += best

        if best == x1:
            _, i = heapq.heappop(q1)
            star[i] = 1
            heapq.heappush(q2, (b[i] - a[i], i))
            heapq.heappush(q4, (-a[i], i))
        elif best == x2:
            _, i = heapq.heappop(q2)
            star[i] = 2
            heapq.heappush(q5, (-(b[i] - a[i]), i))
        elif best == x3:
            _, j = heapq.heappop(q3)
            _, i = heapq.heappop(q4)
            star[j] = 2
            star[i] = 0
        else:
            _, j = heapq.heappop(q3)
            _, i = heapq.heappop(q5)
            star[j] = 2
            star[i] = 1

    print(ans)
    print("".join(map(str, star[:n])))

    sys.stdout = old_stdout
    return out.getvalue().strip()

# sample 1
assert run("2 3\n1 2\n1 2\n").splitlines()[0] == "3"

# minimum size
assert run("1 1\n5 10\n").splitlines()[0] == "5"

# one level, need both stars
assert run("1 2\n5 10\n").splitlines()[0] == "10"

# catches naive star-by-star greedy
assert run("2 2\n3 4\n2 5\n").splitlines()[0] == "4"

# all equal
assert run("3 3\n1 2\n1 2\n1 2\n").splitlines()[0] == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| trị giá`5`| Trường hợp hợp lệ nhỏ nhất | 
|`1 2`| trị giá`10`| Trực tiếp hoàn thành hai sao | 
|`2 2 / (3,4),(2,5)`| trị giá`4`| Trường hợp thất bại vì tham lam ngây thơ | 
| Ba cấp độ giống hệt nhau | trị giá`3`| Tính đối xứng và xử lý ràng buộc | 

## Vỏ cạnh 

Hãy xem xét:```
1 2
5 10
```Nhiệm vụ khả thi duy nhất là trạng thái`2`. Thuật toán đầu tiên mua một sao với chi phí`5`, sau đó nâng cấp nó với giá`5`, đạt tổng chi phí`10`. Kết quả đầu ra đúng ngay cả khi mức này chưa bao giờ được chọn rõ ràng là mức hai sao ngay từ đầu. 

Hãy xem xét ví dụ phản chứng tham lam cổ điển:```
2 2
3 4
2 5
```Một phương pháp ngây thơ chọn chi phí`2`đầu tiên, sau đó là chi phí`3`, cho tổng cộng`5`. Khuôn khổ tham lam hối tiếc cho rằng việc gán trực tiếp cấp độ đầu tiên cho hai sao chỉ tốn kém`4`, cái nào rẻ hơn. Câu trả lời cuối cùng là`4`. 

Hãy xem xét trường hợp trong đó lựa chọn trước đó phải được hoàn tác:```
3 2
1 100
2 3
2 3
```Sau khi lấy chi phí một sao`1`, thuật toán vẫn có thể tự do thay thế lựa chọn đó thông qua quá trình chuyển đổi hối tiếc. Những đống`q4`Và`q5`chính xác là những gì làm cho sự thay thế như vậy có thể thực hiện được. Nếu không có chúng, thuật toán có thể bị mắc kẹt trong cấu hình tối ưu cục bộ nhưng tối ưu toàn cục.
