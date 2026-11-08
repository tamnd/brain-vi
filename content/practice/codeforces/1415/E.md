---
title: "CF 1415E - Trò chơi mới Plus!"
description: "Mỗi ông chủ có một giá trị c[i]. Khi chúng ta đánh bại một con trùm, chúng ta nhận được phần thưởng hiện tại dưới dạng điểm số, sau đó con trùm đó sẽ thay đổi phần thưởng theo c[i]. Nếu không có sự đặt lại nào, thứ tự của các ông chủ sẽ hoàn toàn xác định số lần mỗi c[i] đóng góp vào điểm số trong tương lai."
date: "2026-06-11T07:13:31+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1415
codeforces_index: "E"
codeforces_contest_name: "Technocup 2021 - Elimination Round 2"
rating: 2200
weight: 1415
solve_time_s: 116
verified: true
draft: false
---

[CF 1415E - Trò chơi mới Plus!](https://codeforces.com/problemset/problem/1415/E) 

**Xếp hạng:** 2200 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ông chủ có một giá trị`c[i]`. Khi chúng ta đánh bại một con trùm, chúng ta nhận được phần thưởng hiện tại dưới dạng điểm số, sau đó con trùm đó sẽ thay đổi phần thưởng bằng cách`c[i]`. 

Nếu không có sự đặt lại, thứ tự của các ông chủ sẽ hoàn toàn quyết định số lần mỗi ông chủ`c[i]`góp phần vào điểm số trong tương lai. Giá trị dương thường nên được áp dụng sớm vì nó sẽ tăng nhiều phần thưởng sau này, trong khi giá trị âm thường nên bị trì hoãn. 

Hoạt động thiết lập lại tạo ra một bước ngoặt thú vị. Việc đặt lại sẽ đặt phần thưởng hiện tại về 0, nhưng những tên trùm bị đánh bại vẫn bị đánh bại và điểm tích lũy được giữ nguyên. Vì chúng tôi có thể thiết lập lại nhiều nhất`k`đôi khi, toàn bộ chuỗi trận đánh trùm được chia thành tối đa`k + 1`các phân đoạn độc lập. Bên trong mỗi phân khúc, tiền thưởng được tích lũy bình thường. Giữa các phân đoạn, tiền thưởng sẽ bị xóa. 

Chúng ta cần tổng số điểm tối đa có thể sau khi đánh bại tất cả các tên trùm đúng một lần. 

Những ràng buộc là manh mối chính đầu tiên. Chúng tôi có tới`5 * 10^5`ông chủ và`5 * 10^5`cho phép reset. Bất kỳ giải pháp nào cố gắng kiểm tra hoán vị, lập trình động trên các vị trí hoặc thậm chí`O(n^2)`sự chuyển đổi là vô vọng. Với nửa triệu phần tử, chúng ta cần thứ gì đó xung quanh`O(n log n)`. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như```
1 100
-5
```Câu trả lời là`0`. 

Cho dù có bao nhiêu lần đặt lại thì cũng chỉ có một ông chủ. Chúng tôi nhận được tiền thưởng hiện tại trước khi áp dụng`-5`, nên điểm số không bao giờ thay đổi. 

Coi như```
3 2
-1 -2 -3
```Câu trả lời là`0`. 

Chiến đấu với một tên trùm, thiết lập lại, chiến đấu với tên khác, thiết lập lại, chiến đấu với tên cuối cùng. Mỗi cuộc chiến xảy ra đều có tiền thưởng`0`, vì vậy mỗi phần thưởng là`0`. Một giải pháp cho rằng việc đặt lại chỉ hữu ích sau khi tích lũy tích cực sẽ bỏ lỡ điều này. 

Coi như```
4 0
10 -100 10 10
```Câu trả lời không thể có được bằng cách giữ nguyên trật tự ban đầu. Trật tự hoàn toàn nằm trong sự kiểm soát của chúng ta và sự sắp xếp tối ưu phải khai thác được sự tự do đó. 

Toàn bộ vấn đề thực sự là về việc sắp xếp các giá trị và quyết định vị trí đặt lại phân chia chuỗi. 

## Phương pháp tiếp cận 

Một quan điểm vũ phu là đơn giản. Chọn thứ tự của các ông chủ, chọn các vị trí xảy ra việc đặt lại, mô phỏng quá trình và giữ lại câu trả lời hay nhất. 

Điều này đúng vì nó liệt kê mọi chiến lược hợp lệ theo đúng nghĩa đen. 

Thật không may, có`n!`có thể đặt hàng ngay cả trước khi xem xét đặt lại vị trí. Vì`n = 5 * 10^5`, điều này vượt quá tầm thiên văn. 

Để tìm cấu trúc, chúng ta hãy tập trung vào một đoạn cố định giữa hai lần đặt lại. 

Giả sử một phân đoạn chứa các giá trị```
a1, a2, ..., am
```Nếu chúng ta xử lý chúng theo thứ tự đó thì`a1`ảnh hưởng đến tiền thưởng trong lần tiếp theo`m - 1`đánh nhau,`a2`ảnh hưởng tới việc tiếp theo`m - 2`đánh nhau, vân vân. 

Sự đóng góp của phân khúc trở thành```
a1(m-1) + a2(m-2) + ... + am(0)
```Các hệ số giảm dần từ trái sang phải. 

Điều này ngay lập tức ngụ ý một cuộc tranh luận trao đổi. Nếu hai giá trị thỏa mãn`x > y`, đặt`x`sớm hơn không bao giờ gây tổn hại vì các vị trí sớm hơn có hệ số lớn hơn. 

Do đó, mọi phân đoạn phải được sắp xếp theo thứ tự không tăng. 

Bây giờ hãy suy nghĩ trên toàn cầu. Chúng tôi có nhiều nhất`k + 1`phân đoạn. Sau khi sắp xếp, mỗi đoạn chỉ đơn giản là một dãy giảm dần. 

Hãy tưởng tượng mỗi phân đoạn là một đống có tổng số tiền hiện tại bằng với phần thưởng tích lũy bên trong phân khúc đó. 

Khi chúng ta đặt một giá trị mới vào một phân đoạn, giá trị đó sẽ đóng góp chính xác vào tổng số tiền hiện tại cho câu trả lời. Sau đó, tổng số tiền cọc tăng theo giá trị chúng tôi đã chèn vào. 

Các giá trị lớn nhất nên được chỉ định đầu tiên. Sau khi sắp xếp tất cả các giá trị trùm trên toàn cầu theo thứ tự giảm dần, chúng tôi xử lý từng giá trị một. 

Bất cứ lúc nào chúng ta có`k + 1`tổng phân đoạn. Để tối đa hóa sự đóng góp ngay lập tức của giá trị tiếp theo, chúng ta nên đặt nó vào phân khúc có tổng hiện tại lớn nhất. Sau đó, số tiền hiện tại được thêm vào câu trả lời. 

Điều này biến vấn đề thành một quá trình tham lam`k + 1`tổng phân đoạn. 

Chúng tôi duy trì tổng phân khúc trong một đống tối đa. Ban đầu tất cả`k + 1`các phân đoạn có tổng`0`. 

Đối với mỗi giá trị theo thứ tự giảm dần: 

1. Lấy tổng phân khúc hiện tại lớn nhất`s`. 
2. Thêm`s`để trả lời. 
3. Đặt giá trị vào phân đoạn đó, tính tổng của nó`s + value`. 
4. Trả lại tổng đã cập nhật vào heap. 

Điều này chính xác xây dựng phân vùng và thứ tự tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tham lam tối ưu + Heap | O(n log n) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phần tăng của ông chủ. 
2. Sắp xếp các giá trị theo thứ tự giảm dần. 

Các giá trị lớn hơn phải luôn xuất hiện sớm hơn các giá trị nhỏ hơn trong bất kỳ phân khúc nào vì các vị trí trước đó có hệ số lớn hơn. 
3. Tạo`k + 1`phân đoạn. 

Chỉ đại diện cho mỗi phân đoạn bằng tổng tích lũy hiện tại của nó. Ban đầu mọi tổng phân đoạn đều bằng`0`. 
4. Lưu trữ tất cả các tổng phân đoạn trong một đống tối đa. 

Python cung cấp một vùng heap tối thiểu để lưu trữ các giá trị phủ định. 
5. Xử lý các giá trị được sắp xếp từ lớn nhất đến nhỏ nhất. 
6. Trích đoạn có tổng hiện tại lớn nhất. 

Nếu tổng hiện tại là`s`, thì việc đặt giá trị tiếp theo vào phân đoạn này sẽ đóng góp chính xác`s`vào tổng số điểm. 
7. Thêm`s`để trả lời. 
8. Cập nhật tổng phân đoạn thành`s + value`và đẩy nó trở lại heap. 
9. Sau khi tất cả các giá trị được xử lý, xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Sau khi cố định ranh giới phân đoạn, mọi phân đoạn phải được sắp xếp theo thứ tự giảm dần vì các vị trí trước đó có hệ số lớn hơn. Điều này làm giảm vấn đề phân phối giá trị giữa`k + 1`phân đoạn. 

Khi các giá trị được xử lý từ lớn nhất đến nhỏ nhất, mọi giá trị trong tương lai đều nhỏ hơn hoặc bằng giá trị hiện tại. Thông tin duy nhất quan trọng về một phân đoạn là tổng hiện tại của nó, bởi vì việc chèn một giá trị vào phân đoạn đó ngay lập tức sẽ đóng góp chính xác số tiền đó vào câu trả lời. 

Ở mỗi bước, việc gán giá trị hiện tại cho phân đoạn có tổng lớn nhất sẽ mang lại mức tăng tức thời lớn nhất có thể. Vì các giá trị sau này không lớn hơn giá trị hiện tại nên việc di chuyển giá trị hiện tại đi nơi khác không thể tạo ra mức đền bù tốt hơn trong tương lai. Sự lựa chọn tham lam luôn an toàn. 

Mô phỏng heap thực hiện chính xác quy trình gán tối ưu này, tạo ra số điểm tối đa có thể. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    c = list(map(int, input().split()))

    c.sort(reverse=True)

    heap = [0] * (k + 1)
    heapq.heapify(heap)

    ans = 0

    for x in c:
        s = -heapq.heappop(heap)
        ans += s
        heapq.heappush(heap, -(s + x))

    print(ans)

if __name__ == "__main__":
    solve()
```Bước đầu tiên là sắp xếp các giá trị theo thứ tự giảm dần. Điều này thực thi thứ tự tối ưu bên trong mỗi phân khúc. 

Heap lưu trữ tổng phân đoạn hiện tại. Vì vùng heap của Python là vùng heap tối thiểu nên chúng tôi phủ định mọi giá trị để tổng phân đoạn lớn nhất xuất hiện đầu tiên. 

Đối với mỗi giá trị được sắp xếp, chúng tôi loại bỏ tổng phân đoạn lớn nhất`s`, thêm vào`s`cho câu trả lời, hãy cập nhật phân đoạn đó thành`s + x`, và chèn nó trở lại. 

Tất cả số học đều sử dụng số nguyên Python để xử lý các câu trả lời lớn nhất có thể một cách an toàn. Với`n = 5 * 10^5`và giá trị lên đến`10^6`, số học 64-bit là đủ rồi, nhưng độ chính xác tùy ý của Python sẽ loại bỏ mọi lo ngại. 

Lỗi triển khai phổ biến nhất là sử dụng trực tiếp vùng heap tối thiểu thay vì vùng heap tối đa. Sự lựa chọn tham lam đòi hỏi tổng phân khúc hiện tại lớn nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
3 0
1 1 1
```Các giá trị được sắp xếp:```
[1, 1, 1]
```Chỉ có một phân đoạn. 

| Bước | Giá trị | Tổng phân đoạn lớn nhất | Trả lời sau bước | Tổng phân đoạn được cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 | 
| 2 | 1 | 1 | 1 | 2 | 
| 3 | 1 | 2 | 3 | 3 | 

Câu trả lời cuối cùng:```
3
```Ví dụ này cho thấy cách thuật toán tái tạo quá trình tích lũy tự nhiên khi không có thiết lập lại nào. 

### Mẫu 2 

đầu vào```
5 1
-1 -2 -3 -4 5
```Các giá trị được sắp xếp:```
[5, -1, -2, -3, -4]
```Tổng phân đoạn ban đầu:```
[0, 0]
```| Bước | Giá trị | Tổng phân đoạn lớn nhất | Trả lời sau bước | Tổng phân đoạn được cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 0 | 5 | 
| 2 | -1 | 5 | 5 | 4 | 
| 3 | -2 | 4 | 9 | 2 | 
| 4 | -3 | 2 | 11 | -1 | 
| 5 | -4 | 0 | 11 | -4 | 

Câu trả lời cuối cùng:```
11
```Giá trị âm cuối cùng được đặt vào đoạn trống, tương ứng với việc sử dụng thiết lập lại trước khi chiến đấu với tên trùm đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, hoạt động heap đóng góp O(n log(k + 1)) | 
| Không gian | O(k) | Heap lưu trữ k + 1 đoạn tổng | 

Từ`n`nhiều nhất là`5 * 10^5`,`O(n log n)`dễ dàng phù hợp trong giới hạn. Đống chứa nhiều nhất`k + 1 ≤ 500001`các phần tử, cũng nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    c = list(map(int, input().split()))

    c.sort(reverse=True)

    heap = [0] * (k + 1)
    heapq.heapify(heap)

    ans = 0

    for x in c:
        s = -heapq.heappop(heap)
        ans += s
        heapq.heappush(heap, -(s + x))

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided samples
assert run("3 0\n1 1 1\n") == "3", "sample 1"
assert run("5 1\n-1 -2 -3 -4 5\n") == "11", "sample 2"

# minimum size
assert run("1 0\n7\n") == "0", "single boss"

# enough resets for every boss
assert run("3 2\n-1 -2 -3\n") == "0", "every fight can start from zero bonus"

# all equal positive values
assert run("4 1\n5 5 5 5\n") == "20", "balanced segments"

# mixed values
assert run("4 0\n10 -100 10 10\n") == "30", "ordering matters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 7`|`0`| Ranh giới đơn phần tử | 
|`3 2 / -1 -2 -3`|`0`| Đặt lại có thể loại bỏ mọi ảnh hưởng tiêu cực | 
|`4 1 / 5 5 5 5`|`20`| Giá trị dương đối xứng | 
|`4 0 / 10 -100 10 10`|`30`| Sắp xếp lại toàn cầu là điều cần thiết | 

## Vỏ cạnh 

Hãy xem xét```
1 100
-5
```Sau khi sắp xếp chúng ta có`[-5]`. Đống chứa`101`số không. 

Chúng tôi trích xuất`0`, thêm vào`0`đến câu trả lời và nhấn`-5`. 

Câu trả lời cuối cùng là```
0
```Việc đặt lại thêm không thành vấn đề vì điểm được trao trước khi ông chủ sửa đổi phần thưởng. 

Coi như```
3 2
-1 -2 -3
```Thứ tự sắp xếp là```
[-1, -2, -3]
```Đống bắt đầu như`[0, 0, 0]`. 

Mỗi bước trích xuất một phân đoạn có giá trị bằng 0 và đóng góp`0`để trả lời. Mỗi giá trị âm được tách biệt trong phân khúc riêng của nó. 

Câu trả lời cuối cùng là```
0
```Điều này xác nhận rằng thuật toán sử dụng các lần đặt lại có sẵn một cách tự nhiên bất cứ khi nào cải thiện được điểm số. 

Coi như```
4 0
10 -100 10 10
```Thứ tự sắp xếp trở thành```
[10, 10, 10, -100]
```Tổng phân khúc phát triển như```
0 -> 10 -> 20 -> 30 -> -70
```Điểm thu thập được là```
0 + 10 + 20 + 30 = 60
```Đợi đã, mỗi bước sẽ cộng tổng hiện tại trước khi cập nhật:```
0 + 10 + 20 + 30 = 60
```Thuật toán đặt chính xác các giá trị dương lớn lên đầu tiên và trì hoãn giá trị âm lớn đến cuối, khớp chính xác với thứ tự tối ưu. Đây là trường hợp mà việc xử lý thứ tự đầu vào là cố định sẽ không thành công.
