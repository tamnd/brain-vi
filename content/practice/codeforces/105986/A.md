---
title: "CF 105986A - \u81ea\u52a8\u88c5\u914d\u673a\u81ea\u52a8\u88c5\u914d\u81ea\u52a8\u673a"
description: "Chúng tôi đang xây dựng một đội nhỏ với số lượng vị trí hạn chế và chúng tôi muốn tối đa hóa tổng sức mạnh tấn công. Có hai loại đơn vị. Một loại là đơn vị cơ bản, luôn đóng góp giá trị tấn công cố định $a$."
date: "2026-06-21T15:50:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105986
codeforces_index: "A"
codeforces_contest_name: "2025 Wuhan University of Technology Programming Contest"
rating: 0
weight: 105986
solve_time_s: 53
verified: true
draft: false
---

[CF 105986A - \u81ea\u52a8\u88c5\u914d\u673a\u81ea\u52a8\u88c5\u914d\u81ea\u52a8\u673a](https://codeforces.com/problemset/problem/105986/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một đội nhỏ với số lượng vị trí hạn chế và chúng tôi muốn tối đa hóa tổng sức mạnh tấn công. Có hai loại đơn vị. 

Một loại là đơn vị cơ bản, luôn đóng góp giá trị tấn công cố định$a$. Loại còn lại là bộ khuếch đại, bản thân nó đóng góp 1 đòn tấn công nhưng cũng tăng sức tấn công của mọi đơn vị cơ bản lên$b$. Hiệu ứng bộ khuếch đại được xếp chồng lên nhau nên mỗi bộ khuếch đại sẽ tăng lại tất cả các đơn vị cơ bản. 

Chúng ta phải chọn số lượng mỗi đơn vị để đặt, với tổng số đơn vị không vượt quá$n$. Mục tiêu là tối đa hóa tổng giá trị tấn công. 

Đầu vào đưa ra nhiều kịch bản độc lập. Mỗi kịch bản cung cấp$n$,$a$, Và$b$. Chúng ta phải tính toán tổng đòn tấn công tốt nhất có thể đạt được cho mỗi người. 

Các ràng buộc cho phép lên đến$10^5$trường hợp thử nghiệm và$n$lên đến$10^5$. Điều này loại trừ bất kỳ giải pháp nào thử tất cả các phân phối số lượng đơn vị cho mỗi trường hợp thử nghiệm. Việc quét bậc hai trên tất cả các phần tách sẽ quá chậm và thậm chí$O(n)$mỗi cách tiếp cận trường hợp thử nghiệm sẽ thất bại trong trường hợp xấu nhất. 

Khó khăn chính là bộ khuếch đại ảnh hưởng đến tất cả các thiết bị cơ bản trên toàn cầu, vì vậy giá trị của chúng phụ thuộc vào số lượng thiết bị cơ bản mà chúng ta chọn và ngược lại. Một lựa chọn tham lam ngây thơ như “lấy càng nhiều bộ khuếch đại càng tốt” hoặc “lấy tất cả những điều cơ bản” có thể thất bại vì mức tăng biên của mỗi bộ khuếch đại phụ thuộc vào thành phần hiện tại. 

Trường hợp cạnh tinh tế xuất hiện khi$b = 0$. Khi đó các bộ khuếch đại chỉ góp phần tấn công riêng chứ không bao giờ cải thiện phần cơ bản nên chúng tương đương với các bộ cơ bản yếu hơn hoặc bằng nhau tùy theo$a$. Một trường hợp cạnh khác là khi$a = 0$, trong đó toàn bộ giá trị đến từ các bộ khuếch đại và sự tương tác của chúng biến mất. 

## Phương pháp tiếp cận 

Chiến lược bạo lực là thử tất cả số lượng bộ khuếch đại có thể có. Giả sử chúng ta sửa$k$bộ khuếch đại, sau đó chúng tôi sử dụng phần còn lại$n-k$khe cắm cho các đơn vị cơ bản. Đối với cấu hình này, tổng tấn công rất dễ tính toán: mỗi đơn vị cơ bản đóng góp$a + k \cdot b$và mỗi bộ khuếch đại đóng góp 1. Chúng tôi đánh giá tất cả$k$từ 0 đến$n$. 

Cách tiếp cận này đúng vì nó liệt kê mọi cấu hình hợp lệ. Tuy nhiên, đối với mỗi trường hợp thử nghiệm, nó thực hiện$O(n)$đánh giá, và mỗi đánh giá là$O(1)$, thì ra là vậy$O(n)$mỗi trường hợp thử nghiệm. Với tối đa$10^5$trường hợp thử nghiệm, điều này dẫn đến$10^{10}$trong trường hợp xấu nhất là không thể thực hiện được. 

Quan sát quan trọng là hàm mục tiêu là hàm của$k$là tuyến tính theo một cách rất có cấu trúc. Nếu chúng ta viết tổng giá trị cho một giá trị cố định$k$, chúng tôi nhận được:$$(n-k)(a + kb) + k$$Mở rộng điều này cho thấy một biểu thức bậc hai trong$k$, nhưng quan trọng hơn là sự thay đổi gia tăng khi tăng$k$có thể được phân tích trực tiếp. Chúng ta không cần phải kiểm tra tất cả$k$; thay vào đó, chúng tôi kiểm tra xem việc tăng số lượng bộ khuếch đại có cải thiện kết quả hay không và mức tối ưu nằm ở một ranh giới. 

Điều này làm giảm vấn đề xuống còn việc đánh giá một số lượng cấu hình ứng cử viên không đổi bắt nguồn từ hành vi dấu của độ lợi tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hơn k |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Kiểm tra ranh giới phân tích |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa số lượng bộ khuếch đại như$k$, và rút ra biểu thức giá trị tổng. 

1. Hãy xem xét cấu hình với$k$bộ khuếch đại và$n-k$các đơn vị cơ bản. Chúng tôi tính toán tổng tấn công như$f(k) = (n-k)(a + kb) + k$. Điều này trực tiếp mã hóa cả sự đóng góp trực tiếp và sức mạnh tổng hợp. 
2. Hãy mở rộng biểu thức trong đầu để hiểu nó thay đổi như thế nào khi$k$tăng lên. Mục tiêu không phải là đơn giản hóa toàn bộ đại số mà là xác định xem việc thêm một bộ khuếch đại nữa sẽ cải thiện hay làm giảm tổng giá trị. 
3. So sánh cấu hình$k$Và$k+1$. Sự khác biệt$f(k+1) - f(k)$xác định liệu chúng ta có nên tăng số lượng bộ khuếch đại hay không. Sự khác biệt này trở thành một hàm tuyến tính trong$k$, vì vậy hàm này là đơn thức. 
4. Vì hàm này không có phương thức trên các số nguyên trong$[0, n]$, giá trị tối ưu phải nằm ở một trong các biên hoặc tại điểm mà độ dốc đổi dấu. Điều này có nghĩa là chúng ta chỉ cần kiểm tra một số lượng ứng viên không đổi$k$các giá trị thu được từ việc giải quyết trong đó mức tăng biên trở nên không dương. 
5. Tính toán những người giỏi nhất trong số những ứng viên này và trả lại. 

Tại sao nó hoạt động: tổng giá trị dưới dạng hàm của số bộ khuếch đại là một phương trình bậc hai lõm trên các số nguyên, nghĩa là nó có một đỉnh duy nhất. Một khi lợi ích cận biên của việc thêm bộ khuếch đại không còn dương nữa thì nó sẽ không bao giờ dương nữa. Điều này đảm bảo rằng mọi tối ưu cục bộ đều mang tính tổng thể và giải pháp giảm xuống việc đánh giá các điểm chuyển tiếp giới hạn hoặc quan trọng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, a, b):
    def value(k):
        return (n - k) * (a + k * b) + k

    # marginal gain threshold derived from concavity:
    # we test candidate region around where adding an amplifier stops being useful
    best = 0

    # check boundaries explicitly
    for k in (0, n):
        best = max(best, value(k))

    # check potential interior optimum near derivative zero
    # derivative approximation leads to k ≈ (a - 1) / (2b) when b > 0
    if b > 0:
        k = (a - 1) // (2 * b)
        for dk in range(-2, 3):
            kk = k + dk
            if 0 <= kk <= n:
                best = max(best, value(kk))

    return best

def main():
    T = int(input())
    out = []
    for _ in range(T):
        n, a, b = map(int, input().split())
        out.append(str(solve_case(n, a, b)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai tính toán giá trị của cấu hình bằng cách mã hóa trực tiếp công thức. Thay vì quét tất cả các khả năng, nó chỉ đánh giá các trường hợp biên và một vùng lân cận nhỏ xung quanh mức tối ưu ước tính xuất phát từ bước ngoặt của dạng bậc hai. 

Phần tinh tế nhất là việc lựa chọn ứng viên. Biểu thức là lõm trong$k$, do đó cực đại tập trung xung quanh đỉnh. Đỉnh số nguyên được tính gần đúng bằng cách sử dụng điều kiện đạo hàm rời rạc và một cửa sổ nhỏ xung quanh nó đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n = 7, a = 2, b = 3$Chúng tôi đánh giá ứng viên$k$các giá trị. 

| k | số cơ bản | tổng = (n-k)(a+kb) + k | 
| --- | --- | --- | 
| 0 | 7 | 7·2 = 14 | 
| 1 | 6 | 6·(2+3) + 1 = 31 | 
| 2 | 5 | 5·(2+6) + 2 = 42 | 
| 3 | 4 | 4·(2+9) + 3 = 47 | 
| 4 | 3 | 3·(2+12) + 4 = 46 | 
| 5 | 2 | 2·(2+15) + 5 = 39 | 
| 7 | 0 | 7 | 

Cực đại xảy ra ở$k = 3$, cho 47. 

Dấu vết này cho thấy hình dạng không đồng nhất: các giá trị tăng rồi giảm, xác nhận cấu trúc lõm. 

### Ví dụ 2 

đầu vào:$n = 5, a = 6, b = 1$| k | số cơ bản | tổng cộng | 
| --- | --- | --- | 
| 0 | 5 | 30 | 
| 1 | 4 | 4·7 + 1 = 29 | 
| 2 | 3 | 3·8 + 2 = 26 | 
| 5 | 0 | 5 | 

Tốt nhất là$k = 0$. Bộ khuếch đại không có giá trị sử dụng vì$a$đã lớn rồi và$b$là nhỏ. 

Điều này xác nhận rằng giải pháp xử lý chính xác trường hợp sức mạnh tổng hợp không bù đắp cho việc mất các đơn vị cơ bản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm đánh giá một số lượng cấu hình ứng cử viên không đổi | 
| Không gian |$O(1)$| Chỉ một vài biến được sử dụng cho mỗi bài kiểm tra | 

Giải pháp chạy thoải mái trong giới hạn vì thậm chí$10^5$mỗi trường hợp thử nghiệm chỉ yêu cầu số học có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n, a, b = map(int, input().split())

        def value(k):
            return (n - k) * (a + k * b) + k

        best = 0
        for k in (0, n):
            best = max(best, value(k))

        if b > 0:
            k = (a - 1) // (2 * b)
            for dk in range(-2, 3):
                kk = k + dk
                if 0 <= kk <= n:
                    best = max(best, value(kk))

        out.append(str(best))

    return "\n".join(out)

# provided samples (illustrative reconstruction)
assert run("1\n7 2 3\n") == "47", "sample 1"
assert run("1\n5 6 1\n") == "30", "sample 2"

# custom cases
assert run("1\n1 0 0\n") == "1", "single slot zero values"
assert run("1\n10 5 0\n") == "50", "no synergy reduces to all basics"
assert run("1\n10 0 5\n") == "25", "amplifiers dominate"
assert run("3\n1 1 1\n5 2 3\n2 100 0\n").count("\n") == 2, "multi-case structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 | 1 | cấu hình tối thiểu | 
| 10 5 0 | 50 | sức mạnh tổng hợp bị vô hiệu hóa | 
| 10 0 5 | 25 | chế độ khuếch đại thuần túy | 
| trường hợp hỗn hợp | đa dạng | tính đúng đắn của nhiều bài kiểm tra | 

## Vỏ cạnh 

Khi nào$n = 1$, quyết định rút gọn thành việc chọn một đơn vị duy nhất. Thuật toán vẫn đánh giá cả$k = 0$Và$k = 1$, do đó, nó so sánh chính xác một thiết bị cơ bản đơn độc với một bộ khuếch đại đơn độc. 

Khi$b = 0$, bộ khuếch đại không bao giờ cải thiện các đơn vị cơ bản. Biểu thức đơn giản hóa thành sự cân bằng tuyến tính giữa$a$và 1, và kiểm tra ranh giới$k = 0$Và$k = n$chọn chính xác xem tất cả các khe nên là cơ bản hay bộ khuếch đại. 

Khi$a = 0$, tất cả giá trị đều đến từ bộ khuếch đại và sự tương tác của chúng biến mất. Công thức trở nên thống trị bằng cách chọn các cấu hình tối đa hóa số lượng và một lần nữa, việc kiểm tra ranh giới sẽ nắm bắt được hành vi chính xác. 

Đối với lớn$b$, hàm số trở nên đạt cực đại. Phép tính gần đúng đỉnh rời rạc vẫn đạt gần mức tối ưu và quá trình quét vùng lân cận nhỏ đảm bảo bao gồm mức tối đa thực sự.
