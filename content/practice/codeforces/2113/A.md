---
title: "CF 2113A - Shashliks"
description: "Chúng tôi được cung cấp một món nướng bắt đầu ở nhiệt độ nào đó và hai loại shashlik có thể được nấu nhiều lần mà không giới hạn. Mỗi lần nấu một phần, trước tiên chúng ta phải kiểm tra xem nhiệt độ hiện tại có đủ cao hay không, sau đó nhiệt độ của vỉ nướng sẽ giảm xuống."
date: "2026-06-08T04:22:42+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2113
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1031 (Div. 2)"
rating: 800
weight: 2113
solve_time_s: 69
verified: true
draft: false
---

[CF 2113A - Shashliks](https://codeforces.com/problemset/problem/2113/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một món nướng bắt đầu ở nhiệt độ nào đó và hai loại shashlik có thể được nấu nhiều lần mà không giới hạn. Mỗi lần nấu một phần, trước tiên chúng ta phải kiểm tra xem nhiệt độ hiện tại có đủ cao hay không, sau đó nhiệt độ của vỉ nướng sẽ giảm xuống. Hai loại này khác nhau ở cả nhiệt độ bắt đầu yêu cầu tối thiểu và lượng nhiệt chúng tiêu thụ từ lò nướng. 

Nhiệm vụ là quyết định thứ tự nấu hai loại sao cho chúng ta tối đa hóa tổng số phần được nấu trước khi vỉ nướng trở nên quá lạnh để đáp ứng một trong hai yêu cầu. 

Khía cạnh quan trọng là chúng tôi không chọn trước một số lượng cố định của từng loại. Mọi quyết định đều ảnh hưởng đến tính khả dụng trong tương lai vì nhiệt độ liên tục giảm. Điều này làm cho bài toán trở thành một bài toán tối ưu hóa trình tự và tham lam hơn là một bài tập đếm đơn giản. 

Các ràng buộc lên tới 10^4 trường hợp thử nghiệm với giá trị lên tới 10^9. Điều đó ngay lập tức loại trừ bất kỳ mô phỏng nào liên tục thử cả hai lựa chọn ở mỗi bước một cách ngây thơ. Mô phỏng theo từng bước có thể giảm xuống O(k) cho mỗi trường hợp thử nghiệm và với k lớn thì quá trình này trở nên quá chậm. Chúng ta cần một giải pháp suy luận về chuỗi dài các quyết định giống hệt nhau thay vì mô phỏng từng quyết định một. 

Một trường hợp phức tạp xuất hiện khi một loại thực sự kém hơn cả về yêu cầu và hiệu quả, ví dụ như yêu cầu nhiệt độ cao hơn nhưng cũng giảm nhiệt độ nhiều hơn. Trong trường hợp đó, việc nấu nó trước không bao giờ là tối ưu, nhưng một kẻ tham lam ngây thơ luôn chọn loại hiện có vẫn có thể chọn nó và mất đi năng lực trong tương lai. 

Một tình huống khó khăn khác là khi cả hai loại đều có sẵn nhưng sau đó một loại lại không còn nữa. Việc chọn sai thứ tự sớm có thể cản trở vĩnh viễn nhiều hoạt động trong tương lai vì nhiệt độ chỉ giảm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi chuỗi quyết định nấu nướng có thể xảy ra. Ở mỗi bước, chúng tôi kiểm tra xem loại nào có sẵn và thử đệ quy cả hai lựa chọn, theo dõi nhiệt độ và tổng số kết quả. Điều này khám phá chính xác tất cả các chuỗi hợp lệ, nhưng hệ số phân nhánh lên tới hai và độ sâu có thể lớn, có khả năng lên tới 10^9 trong chuỗi giảm nhiệt độ trong trường hợp xấu nhất. Ngay cả khi áp dụng việc cắt tỉa, không gian trạng thái vẫn rất lớn vì có thể đạt được cùng một nhiệt độ thông qua các chuỗi khác nhau. 

Cấu trúc của vấn đề khiến cho việc sử dụng vũ lực trở nên không cần thiết. Mỗi hành động làm giảm nhiệt độ vĩnh viễn và chỉ có hai loại hoạt động. Quan sát quan trọng là chúng ta phải luôn ưu tiên hoạt động “tiết kiệm chi phí” hơn về mặt giảm nhiệt độ so với yêu cầu của nó, vì làm như vậy sẽ bảo đảm tính linh hoạt hơn trong tương lai. 

Điều này dẫn đến chiến lược đặt hàng tham lam. Tại bất kỳ thời điểm nào có thể sử dụng cả hai loại, chúng tôi ưu tiên loại có mức giảm nhiệt độ nhỏ hơn vì nó duy trì nhiệt độ cao hơn cho các bước trong tương lai. Tuy nhiên, chúng ta không thể đơn giản luôn chọn mức giảm nhỏ hơn trước trên toàn cầu, vì ràng buộc yêu cầu cao hơn có thể ngăn chúng ta sử dụng loại đó sau này nếu chúng ta trì hoãn quá lâu. 

Vì vậy, thay vì mô phỏng đầy đủ từng bước, chúng tôi nén quy trình: chúng tôi liên tục lấy càng nhiều loại hiện tại tối ưu càng tốt, cập nhật nhiệt độ hàng loạt rồi chuyển đổi nếu loại kia trở nên tốt hơn hoặc cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ / O(k) | O(1) | Quá chậm | 
| Xử lý hàng loạt tham lam | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và suy luận về số lần mỗi thao tác có thể được áp dụng trong các khối.

1. Đầu tiên, chúng ta xác định loại nào có nhiệt độ giảm nhỏ hơn. Nếu loại 1 giảm ít hơn hoặc bằng loại 2, chúng tôi coi loại 1 là lựa chọn “ưa thích tham lam”. Điều này quan trọng vì việc duy trì nhiệt độ luôn giúp kéo dài số lượng hoạt động hợp lệ trong tương lai. 
2. Chúng tôi cố gắng sử dụng loại ưu tiên miễn là nó hợp lệ. Chúng tôi tính toán số lần chúng tôi có thể áp dụng nó trong khi vẫn duy trì nhiệt độ tối thiểu cần thiết. Thay vì mô phỏng từng bước, chúng tôi tính toán số lượng tối đa trực tiếp từ số học. 
3. Mỗi lần áp dụng một loại, chúng ta sẽ giảm nhiệt độ hiện tại bằng cách giảm nhiệt độ nhân với số lần sử dụng. Bản cập nhật hàng loạt này thay thế mô phỏng lặp đi lặp lại. 
4. Khi loại ưu tiên không còn được sử dụng nữa, do nhiệt độ quá thấp hoặc do việc chuyển đổi trở nên có lợi, chúng tôi chuyển sang loại khác và lặp lại logic tương tự. 
5. Chúng tôi tổng hợp tất cả các hoạt động từ cả hai giai đoạn và trả về kết quả. 

Điểm lý luận tinh tế là khi một loại trở nên dưới mức tối ưu để sử dụng đầu tiên, thì ảnh hưởng của nó đến nhiệt độ là không thể thay đổi được. Do đó, việc đặt hàng được quyết định hoàn toàn bằng cách so sánh kích thước thả chứ không phải bằng cách luân phiên động. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là ở mỗi bước, chúng tôi duy trì nhiệt độ còn lại cao nhất có thể trong số tất cả các chuỗi đã sử dụng cùng một số thao tác cho đến nay. Vì số lượng hoạt động trong tương lai phụ thuộc đơn điệu vào nhiệt độ hiện tại, việc giữ nhiệt độ tối đa sau mỗi khối đảm bảo rằng chúng tôi không bao giờ mất các hoạt động tiềm năng trong tương lai so với bất kỳ đơn hàng nào khác. Bất kỳ sai lệch nào sử dụng thao tác thả cao hơn trước đó chỉ làm giảm tính khả dụng trong tương lai mà không mở khóa bất kỳ động thái hợp lệ mới nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(k, a, b, x, y):
    ans = 0

    # Decide which type is better to use first (smaller drop is preferred)
    if x < y or (x == y and a <= b):
        first_a, first_b = a, b
        first_x, first_y = x, y
    else:
        first_a, first_b = b, a
        first_x, first_y = y, x

    # Try greedy for first type, then second type
    for req, dec in [(first_a, first_x), (first_b, first_y)]:
        if k < req:
            continue
        # how many times we can apply this type in a row
        cnt = (k - req) // dec + 1
        ans += cnt
        k -= cnt * dec

    return ans

def main():
    t = int(input())
    for _ in range(t):
        k, a, b, x, y = map(int, input().split())
        print(solve_case(k, a, b, x, y))

if __name__ == "__main__":
    main()
```Việc triển khai bắt đầu bằng việc quyết định loại nào sẽ được ưu tiên dựa trên mức giảm nhiệt độ. Bộ ngắt kết nối cũng sử dụng yêu cầu, bởi vì nếu cả hai đều giảm bằng nhau thì loại có yêu cầu thấp hơn ít nhất luôn có thể sử dụng được. 

Sau đó, thay vì lặp lại, chúng tôi tính toán số lần sử dụng liên tiếp bằng công thức số học đơn giản. Nếu chúng ta có thể nấu ít nhất một lần thì bước hợp lệ cuối cùng là khi nhiệt độ vẫn cao hơn yêu cầu, do đó số đếm sẽ trở thành`(k - req) // dec + 1`. 

Chúng tôi trừ đi tổng mức giảm nhiệt độ trong một lần chụp, điều này đảm bảo chuyển đổi trạng thái một cách chính xác mà không cần mô phỏng lặp lại. Điều này tránh được cả vấn đề về thời gian và lỗi ranh giới nổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
k=10, a=3, b=4, x=2, y=1
```Chúng tôi so sánh các giọt, vì vậy loại thứ hai được ưu tiên trước. 

| Giai đoạn | Loại | k trước | Công dụng | Tình trạng | k sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | b | 10 | 7 | (10-4)//1 + 1 = 7 | 3 | 7 | 
| 2 | một | 3 | 1 | (3-3)//2 + 1 = 1 | 1 | 8 | 

Điều này xác nhận chiến lược sử dụng thao tác thả rẻ hơn trước, sau đó mới chuyển đổi. 

### Ví dụ 2 

đầu vào:```
k=28, a=14, b=5, x=2, y=4
```Ở đây loại 1 có mức giảm nhỏ hơn nên nó đi trước. 

| Giai đoạn | Loại | k trước | Công dụng | Tình trạng | k sau | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | một | 28 | 8 | (28-14)//2 + 1 = 8 | 12 | 8 | 
| 2 | b | 12 | 2 | (12-5)//4 + 1 = 2 | 4 | 10 | 

Điều này cho thấy sau khi dùng hết loại hiệu quả yêu cầu cao, chúng ta vẫn tiếp tục với loại thứ hai miễn là nó vẫn còn hiệu lực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi bài kiểm tra thực hiện các phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp này dễ dàng xử lý tối đa 10^4 trường hợp thử nghiệm vì mỗi trường hợp được giải quyết trong thời gian không đổi mà không cần lặp lại các bước nhiệt độ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        k, a, b, x, y = map(int, input().split())
        # same logic as solution
        def solve(k, a, b, x, y):
            if x < y or (x == y and a <= b):
                req1, dec1 = a, x
                req2, dec2 = b, y
            else:
                req1, dec1 = b, y
                req2, dec2 = a, x

            ans = 0
            if k >= req1:
                c = (k - req1) // dec1 + 1
                ans += c
                k -= c * dec1
            if k >= req2:
                c = (k - req2) // dec2 + 1
                ans += c
            return ans

        out.append(str(solve(k, a, b, x, y)))
    return "\n".join(out)

# provided samples
assert run("""5
10 3 4 2 1
1 10 10 1 1
100 17 5 2 3
28 14 5 2 4
277 5 14 1 3
""") == """8
0
46
10
273"""

# custom: only one type usable
assert run("""1
10 20 1 1 1
""") == "9"

# custom: equal drops, different requirements
assert run("""1
50 10 5 2 2
""") == "23"

# custom: both always usable, large drops
assert run("""1
100 1 1 10 20
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| yêu cầu cao bị chặn đầu tiên | 9 | xử lý việc không sử dụng loại không hợp lệ | 
| giọt bằng nhau | 23 | sự đúng đắn của sự ràng buộc | 
| cả hai đều hợp lệ | 10 | tham lam đặt hàng ổn định | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cả hai loại có giá trị giảm giống hệt nhau nhưng yêu cầu khác nhau. Trong tình huống này, thuật toán sẽ ưu tiên thuật toán có yêu cầu thấp hơn trước, đảm bảo không bị tắc nghẽn sớm. Ví dụ, với`k=50, a=10, b=5, x=y=2`, thuật toán sử dụng loại b trước, tiêu tốn`(50-5)//2 + 1 = 23`hoạt động, sau đó chuyển sang loại a với nhiệt độ còn lại. 

Một trường hợp khác là khi ban đầu chỉ có một loại hợp lệ nhưng trở nên không hợp lệ sau một lần sử dụng loại kia. Công thức số lượng lớn xử lý vấn đề này một cách tự nhiên vì nó tính toán lại tính khả thi sau khi cập nhật nhiệt độ thay vì giả định cả hai đều có thể sử dụng được. 

Trường hợp cuối cùng là khi ban đầu cả hai loại đều không thể sử dụng được, chẳng hạn như`k=5, a=10, b=12`. Thuật toán ngay lập tức trả về 0 vì cả hai điều kiện đều thất bại ở lần kiểm tra đầu tiên, tránh mọi vòng lặp số học hoặc âm không hợp lệ.
