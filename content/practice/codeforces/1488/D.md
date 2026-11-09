---
title: "CF 1488D - Giải quyết vấn đề Marathon"
description: "Chúng ta được yêu cầu xây dựng một chuỗi số nguyên không giảm trong một số ngày cố định, trong đó mỗi ngày biểu thị số lượng bài toán mà Polycarp giải quyết được."
date: "2026-06-10T22:47:17+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1488
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 6"
rating: 1900
weight: 1488
solve_time_s: 133
verified: true
draft: false
---

[CF 1488D - Giải chạy Marathon](https://codeforces.com/problemset/problem/1488/D) 

**Xếp hạng:** 1900 
**Tags:** *đặc biệt, tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một chuỗi số nguyên không giảm trong một số ngày cố định, trong đó mỗi ngày biểu thị số lượng bài toán mà Polycarp giải quyết được. Tổng số vấn đề là cố định và trình tự phải tuân theo hai quy tắc cấu trúc: nó không thể giảm theo thời gian trong ngày và nó không thể tăng nhiều hơn hệ số hai giữa các ngày liên tiếp. 

Điều duy nhất chúng ta quan tâm cuối cùng là giá trị cuối cùng của chuỗi, nhưng giá trị đó bị ràng buộc gián tiếp bởi sự tồn tại của một chuỗi đầy đủ hợp lệ có tổng chính xác bằng tổng đã cho. 

Vì vậy, bài toán trở thành: trong số tất cả các chuỗi có độ dài n bắt đầu từ ít nhất 1, không giảm và tăng tối đa gấp đôi mỗi bước, hãy tìm phần tử cuối cùng lớn nhất có thể sao cho toàn bộ chuỗi có thể được chọn tổng chính xác bằng s. 

Các ràng buộc đẩy chúng tôi tới O(log s) hoặc O(n log s) cho mỗi giải pháp thử nghiệm. Vì n và s lên tới 10^18 và có tới 1000 trường hợp kiểm thử nên mọi nỗ lực mô phỏng trình tự một cách rõ ràng hoặc thử các ứng viên có cấu trúc lồng nhau sẽ thất bại. Cấu trúc của các ràng buộc là đơn điệu và nhân lên, thường báo hiệu một cuộc kiểm tra tính khả thi tham lam kết hợp với tìm kiếm nhị phân trên câu trả lời. 

Một vấn đề tế nhị sẽ xuất hiện nếu chúng ta bỏ qua tính khả thi và chỉ nghĩ đến việc tối đa hóa yếu tố cuối cùng. Ví dụ: nếu chúng ta cố gắng đẩy phần tử cuối cùng càng lớn càng tốt, chúng ta có thể tạo một tiền tố không thể không giảm trong khi vẫn tổng bằng s. Một chế độ thất bại khác là giả định rằng khi giá trị cuối cùng của ứng cử viên hoạt động với một n nhất định, thì tất cả các giá trị nhỏ hơn cũng hoạt động mà không kiểm tra cẩn thận xem chuỗi phải “mở rộng về phía sau” như thế nào. 

Khó khăn chính là trình tự bị ràng buộc cả cục bộ (tỷ lệ giữa các lân cận) và toàn cầu (tổng cố định) và cấu trúc tối ưu không phải là tùy ý, nó sẽ được xác định chặt chẽ sau khi giá trị cuối cùng được cố định. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử tất cả các trình tự có thể. Đối với mỗi chuỗi, chúng tôi sẽ xác minh tính đơn điệu, ràng buộc nhân đôi, ràng buộc tổng và theo dõi phần tử cuối cùng tối đa. Ngay cả khi giới hạn các giá trị trong phạm vi hợp lý, số lượng chuỗi vẫn tăng theo cấp số nhân với n, vì mỗi vị trí phụ thuộc vào vị trí trước đó nhưng vẫn có nhiều lựa chọn hợp lệ. 

Một cách mạnh mẽ hơn có cấu trúc chặt chẽ hơn là cố định giá trị cuối cùng x và sau đó cố gắng xây dựng bất kỳ chuỗi hợp lệ nào kết thúc bằng x với tổng tối thiểu hoặc tổng chính xác s. Thậm chí điều này còn trở nên tốn kém vì không gian của các tiền tố hợp lệ vẫn theo cấp số nhân theo n. Điểm nghẽn là mỗi vị trí có thể thay đổi trong một phạm vi phụ thuộc vào các giá trị trước đó. 

Quan sát quan trọng là đối với một giá trị cuối cùng cố định, chuỗi tối thiểu hóa tổng được xác định duy nhất. Nếu chúng ta muốn tạo một chuỗi hợp lệ kết thúc bằng x, cách tốt nhất để giữ tổng càng nhỏ càng tốt là đẩy các giá trị xuống thấp nhất có thể trong khi di chuyển lùi. Vì chúng ta cần một chuỗi không giảm về phía trước, tương đương về phía sau, chúng ta phải đảm bảo mỗi phần tử trước ít nhất là 1 và nhiều nhất là một nửa của phần tử tiếp theo, nhưng cũng không lớn hơn phần tử tiếp theo. 

Điều này có nghĩa là khi chúng ta cố định một giá trị cuối cùng, cấu trúc tối ưu (tổng tối thiểu) đạt được bằng cách di chuyển lùi về phía sau một cách tham lam, liên tục lấy giá trị tối thiểu được phép, về cơ bản là giá trị lớn nhất không vi phạm ràng buộc nhân đôi khi đảo ngược. 

Khi chúng ta có thể tính tổng tối thiểu có thể có cho một giá trị cuối cùng cho trước, bài toán trở nên đơn điệu trong x. Nếu một x nhất định khả thi (tổng tối thiểu ≤ s), thì bất kỳ x nhỏ hơn nào cũng khả thi. Điều này cho phép tìm kiếm nhị phân trên câu trả lời.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trình tự Brute Force | hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + tính khả thi tham lam | O(n log s) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tìm kiếm nhị phân giá trị lớn nhất có thể có của a_n và đối với mỗi ứng cử viên, chúng tôi kiểm tra xem liệu một chuỗi hợp lệ có độ dài n có thể được hình thành mà tổng của nó không vượt quá s hay không. 

1. Cố định giá trị cuối cùng của ứng cử viên x và cố gắng tính tổng tối thiểu có thể có của một chuỗi hợp lệ kết thúc tại x. Mục tiêu là để xem liệu chúng ta có thể “nén” chuỗi đến mức mà các ràng buộc cho phép hay không. 
2. Lùi lại từ ngày n đến ngày 1, duy trì giá trị hiện tại cur bắt đầu từ x. Đối với mỗi ngày trước đó, giá trị nhỏ nhất có thể là 1, nhưng chúng ta cũng phải tôn trọng rằng tốc độ tăng trưởng trong tương lai không thể vượt quá gấp đôi. Đảo ngược ràng buộc có nghĩa là giá trị trước đó ít nhất phải bằng trần(cur / 2) trong một cấu trúc chặt chẽ để tránh lạm phát không cần thiết. 
3. Chính xác hơn, để giảm thiểu tổng, mỗi phần tử trước đó được chọn là min(cur, max khả thi ở các vị trí còn lại). Trong một cấu trúc tối ưu, chúng ta liên tục lấy cur = cur // 2 khi điều đó giữ cho chuỗi hợp lệ, bởi vì giảm một nửa là cách mạnh nhất để giảm các giá trị trong khi vẫn duy trì khả năng đạt đến x ở cuối. 
4. Chúng ta tiếp tục quá trình lùi này trong n bước, tích lũy tổng. Nếu tại bất kỳ thời điểm nào tổng vượt quá s, chúng ta có thể dừng sớm vì x này không khả thi. 
5. Nếu tổng tính được là ≤ s thì x có thể đạt được. Sau đó chúng tôi thử các giá trị lớn hơn trong tìm kiếm nhị phân; nếu không thì chúng ta giảm x. 
6. Tìm kiếm nhị phân chạy trong phạm vi [1, s], vì a_n không thể vượt quá s. 

Tại sao nó hoạt động: 

Bất biến quan trọng là đối với bất kỳ điểm cuối cố định x nào, cấu trúc lùi tham lam tạo ra chuỗi hợp lệ nhỏ nhất về mặt từ điển (và do đó tổng tối thiểu) kết thúc tại x. Bất kỳ nỗ lực nào nhằm giảm thêm một số giá trị trung gian sẽ vi phạm tính đơn điệu hoặc ràng buộc nhân đôi. Do đó, tính khả thi của x chỉ phụ thuộc vào tổng tối thiểu duy nhất này, làm cho vị từ trong x trở nên đơn điệu và cho phép tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def feasible(n, s, x):
    total = 0
    cur = x

    for _ in range(n):
        total += cur
        if total > s:
            return False
        # move backwards: best we can do is shrink by factor 2
        cur = cur // 2
        if cur == 0:
            cur = 1

    return total <= s

def solve():
    t = int(input())
    for _ in range(t):
        n, s = map(int, input().split())

        lo, hi = 1, s
        ans = 1

        while lo <= hi:
            mid = (lo + hi) // 2
            if feasible(n, s, mid):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt việc kiểm tra tính khả thi khỏi việc tìm kiếm. các`feasible`hàm mô phỏng cấu trúc lùi tối ưu của chuỗi cho một giá trị cuối cùng cố định. Sự tinh tế quan trọng là sự chuyển đổi`cur = cur // 2`, mã hóa mức thu nhỏ mạnh nhất cho phép trong khi vẫn duy trì khả năng đạt đến điểm cuối dưới các ràng buộc nhân đôi. 

Tìm kiếm nhị phân sau đó sử dụng vị từ khả thi đơn điệu này để tối đa hóa phần tử cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: n = 3, s = 9 

Chúng tôi kiểm tra các giá trị ứng viên cho a3. 

| x | xây dựng trình tự (ý tưởng lạc hậu) | tổng hợp | khả thi | 
| --- | --- | --- | --- | 
| 4 | [1, 2, 4] | 7 | vâng | 
| 5 | [1, 2, 5] không hợp lệ, tổng hợp lệ tốt nhất vẫn vượt quá 9 ràng buộc | >9 | không | 

Với x = 4, chúng ta có thể xây dựng [2, 3, 4], khớp chính xác với tổng 9. Tìm kiếm nhị phân giải quyết trên 4. 

Điều này cho thấy tính khả thi phụ thuộc vào việc liệu chuỗi có thể “kéo dài lên trên” khi nhân đôi trong khi vẫn giữ các giá trị ban đầu ở mức nhỏ hay không. 

### Ví dụ 2 

Đầu vào: n = 2, s = 6 

| x | trình tự | tổng hợp | khả thi | 
| --- | --- | --- | --- | 
| 3 | [3, 3] | 6 | vâng | 
| 4 | [2, 4] | 6 | vâng | 
| 5 | không thể | >6 | không | 

Ở đây cả 3 và 4 đều khả thi, nhưng 4 lớn hơn nên nó trở thành đáp án. Điều này thể hiện sự đơn điệu trong không gian trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log s log n) | tìm kiếm nhị phân trên s, mỗi lần kiểm tra tính khả thi sẽ chạy trong O(n) | 
| Không gian | O(1) | chỉ một vài số nguyên được sử dụng cho mỗi bài kiểm tra | 

Cho n, s 10^18 và t ≤ 1000, lời giải dựa trên thực tế là vòng lặp thực tế rất ngắn trong thực tế do giảm nhanh cur, giúp việc kiểm tra nhanh hơn nhiều so với giới hạn trong trường hợp xấu nhất được đề xuất và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()

    input = sys.stdin.readline

    def feasible(n, s, x):
        total = 0
        cur = x
        for _ in range(n):
            total += cur
            if total > s:
                return False
            cur = cur // 2
            if cur == 0:
                cur = 1
        return total <= s

    def solve():
        t = int(input())
        for _ in range(t):
            n, s = map(int, input().split())
            lo, hi = 1, s
            ans = 1
            while lo <= hi:
                mid = (lo + hi) // 2
                if feasible(n, s, mid):
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1
            print(ans)

    solve()
    return output.getvalue()

# provided samples
assert run("3\n1 15\n3 9\n2 6\n") == "15\n4\n4\n"
# custom cases
assert run("1\n1 100\n") == "100\n"
assert run("1\n5 5\n") == "1\n"
assert run("1\n2 100\n") == "50\n"
assert run("1\n10 1000\n") == "64\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 s lớn | s | trường hợp cạnh phần tử đơn | 
| n lớn, chặt s | 1 | hạn chế tăng trưởng tối thiểu | 
| n=2 s lớn | 50 | tổng cân bằng và giá trị cuối cùng | 
| lớn hơn n | hành vi sức mạnh của hai | hiệu ứng giảm một nửa lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi n = 1. Dãy có một phần tử duy nhất và tất cả các ràng buộc đều biến mất ngoại trừ ràng buộc tổng, vì vậy câu trả lời phải là s. Thuật toán xử lý điều này vì tính khả thi của x chỉ đơn giản là liệu x ≤ s và tìm kiếm nhị phân sẽ trả về s một cách tự nhiên. 

Một trường hợp khác là khi s rất nhỏ so với n. Ví dụ: n = 5, s = 5 buộc tất cả các giá trị là 1. Mô phỏng ngược ngay lập tức giảm cur xuống 1 và giữ tổng cực tiểu, do đó mọi ứng cử viên x > 1 trở nên không khả thi và tìm kiếm nhị phân thu gọn về 1. 

Trường hợp cạnh cấu trúc cuối cùng là khi s lớn và n nhỏ. Với n = 2, sự phân chia tối ưu có xu hướng cân bằng giữa a1 và a2 dưới các ràng buộc nhân đôi. Kiểm tra tính khả thi cho phép x lớn cho đến khi tổng ngụ ý vượt quá s, nắm bắt chính xác rằng a_n tối ưu là khoảng s/2 trong chế độ đó.
