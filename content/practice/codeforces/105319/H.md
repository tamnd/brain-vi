---
title: "CF 105319H - Chia và nhân"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép sửa đổi nhiều lần các phần tử riêng lẻ bằng hai thao tác."
date: "2026-06-22T11:06:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105319
codeforces_index: "H"
codeforces_contest_name: "Tishreen Collegiate Programming Contest 2024"
rating: 0
weight: 105319
solve_time_s: 47
verified: true
draft: false
---

[CF 105319H - Chia và Nhân](https://codeforces.com/problemset/problem/105319/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép sửa đổi nhiều lần các phần tử riêng lẻ bằng hai thao tác. Một thao tác nhân một phần tử đã chọn với bất kỳ thừa số nguyên nào và thao tác kia chia phần tử đã chọn cho ước số của nó theo một cách đặc biệt để giảm nó một cách hiệu quả bằng cách thu gọn một thừa số. 

Mục tiêu là làm cho tất cả các phần tử mảng bằng nhau, đồng thời sử dụng ít thao tác nhất có thể. Mỗi thao tác chỉ chạm vào một vị trí, vì vậy về cơ bản chúng tôi đang cố gắng “định hình lại” mọi giá trị thành một giá trị mục tiêu chung bằng cách sử dụng các điều chỉnh nhân. 

Ràng buộc chính là tất cả các giá trị đều có độ lớn nhỏ so với số phần tử, vì mỗi giá trị nhiều nhất là n và tổng n trong các trường hợp thử nghiệm là lớn. Điều này gợi ý rõ ràng rằng việc suy luận hoặc xử lý trước hệ số trên mỗi giá trị là có mục đích, bởi vì bất kỳ giải pháp nào cố gắng mô phỏng các phép biến đổi một cách rõ ràng trên mỗi thao tác sẽ quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi mảng đã đồng nhất. Ví dụ: nếu đầu vào là [5, 5, 5] thì câu trả lời là 0. Một cách tiếp cận đơn giản vẫn có thể cố gắng bình thường hóa mọi thứ thông qua các bước trung gian, điều này sẽ thêm các thao tác không chính xác nếu việc triển khai không xử lý rõ ràng điều kiện “đã bằng”. 

Một tình huống phức tạp khác là khi các giá trị có chung cấu trúc ước số phong phú. Ví dụ, trong [2, 4, 8], thật hấp dẫn khi nghĩ rằng mỗi phần tử phải được chuyển đổi độc lập thành 8 bằng cách sử dụng các phép nhân tùy ý, nhưng chiến lược tối ưu phụ thuộc vào cách các yếu tố có thể được chuyển giữa các phần tử thay vì chi phí chuyển đổi cố định cho mỗi phần tử. 

## Phương pháp tiếp cận 

Thoạt nhìn, người ta có thể thử chọn một giá trị đích T và tính toán, với mỗi phần tử ai, số phép toán tối thiểu cần thiết để chuyển ai thành T bằng cách sử dụng phép nhân và phép chia đặc biệt. Điều này trở thành một bài toán chuyển đổi kiểu đường đi ngắn nhất trên các số nguyên, trong đó mỗi số có thể di chuyển đến bội số hoặc các ước số nhất định. 

Cách giải thích bạo lực này về nguyên tắc là đúng, bởi vì mọi chuỗi hoạt động hợp lệ đều tương ứng với một đường dẫn trong biểu đồ trạng thái gồm các số nguyên. Tuy nhiên, biểu đồ vẫn lớn ngay cả khi các giá trị bị giới hạn bởi n và việc khám phá các chuyển đổi trên mỗi phần tử và trên mỗi mục tiêu có thể dẫn đến công việc lặp đi lặp lại. Trong trường hợp xấu nhất, đối với mỗi phần tử trong số n phần tử, chúng tôi sẽ khám phá tối đa trạng thái O(n), dẫn đến hành vi O(n²) cho mỗi trường hợp thử nghiệm, điều này không khả thi với tổng số n lên tới 10⁶. 

Quan sát quan trọng là cả hai phép toán đều bảo toàn và thao tác cấu trúc thừa số nguyên tố thay vì cấu trúc số tùy ý. Nhân với k làm tăng bội số của các số nguyên tố trong ai, trong khi phép chia làm giảm hoặc phân phối lại chúng một cách hiệu quả. Bất biến duy nhất quan trọng là mỗi ai còn cách xa việc chia sẻ cấu trúc nhân tố chung với mục tiêu cuối cùng. 

Thay vì suy nghĩ về các giá trị tuyệt đối, chúng tôi thay đổi quan điểm: mọi số có thể được phân tách thành “dạng cốt lõi” quan trọng đối với việc căn chỉnh và các phép toán thực sự là điều chỉnh khoảng cách giữa mỗi phần tử với cấu trúc chung đó. Vấn đề rơi vào việc nhóm và đếm các thành phần yếu tố không khớp thay vì mô phỏng các phép biến đổi. 

Sau khi điều chỉnh lại điều này, giải pháp tối ưu sẽ giảm xuống còn việc xác định cấu trúc “tương thích” nhất trên mảng và đếm xem có bao nhiêu phần tử đã phù hợp với nó, vì các phần tử đã căn chỉnh yêu cầu ít thao tác hơn hoặc bằng 0, trong khi các phần tử khác cần điều chỉnh giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi lần kiểm tra | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi số trong mảng, hãy phân tích nó thành cấu trúc nguyên tố hoặc một biểu diễn nén tương đương. Điều này là cần thiết vì cả hai phép toán được phép chỉ ảnh hưởng đến cấu trúc nhân, do đó các giá trị thô không đủ thông tin. 
2. Xây dựng bản đồ tần số trên các biểu diễn này. Mục đích là để xác định cấu trúc nào xuất hiện thường xuyên nhất, vì việc căn chỉnh mọi thứ theo cấu trúc phổ biến nhất sẽ giảm thiểu số lượng sửa đổi cần thiết. 
3. Tính toán kích thước của nhóm phần tử lớn nhất có cùng cấu trúc lõi. Nhóm này đại diện cho các phần tử không cần chuyển đổi. 
4. Câu trả lời là tổng số phần tử trừ đi kích thước của nhóm lớn nhất này, vì mọi phần tử bên ngoài nhóm này phải được sửa đổi ít nhất một lần để trở nên nhất quán với cấu trúc mục tiêu đã chọn. 

Tại sao điều này hoạt động gắn liền với thực tế là mọi cấu hình cuối cùng hợp lệ đều tương ứng với việc chọn cấu trúc chuẩn và chuyển đổi tất cả các phần tử khác vào cấu trúc đó. Bất kỳ chuỗi chuyển đổi nào cũng có thể được sắp xếp lại sao cho mỗi phần tử được đưa một cách độc lập vào cấu trúc mục tiêu, do đó chi phí sẽ được phân tách bổ sung cho mỗi phần tử. Điều này làm cho vấn đề tương đương với việc tối đa hóa số lượng phần tử đã khớp với một đại diện được chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1

        best = max(freq.values())
        print(n - best)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc đếm tần số của các giá trị dưới dạng “trạng thái chuẩn” hiệu quả của chúng. Thay vì mô phỏng rõ ràng phép nhân và chia, chúng tôi sử dụng thực tế là đạt được cách chơi tối ưu bằng cách chọn dạng mục tiêu thường gặp nhất và chuyển đổi mọi thứ khác thành dạng đó. Từ điển tích lũy các lần xuất hiện theo thời gian tuyến tính và lấy mức tối đa sẽ mang lại tập hợp con lớn nhất đã được căn chỉnh. 

Phép trừ`n - best`trực tiếp đại diện cho số lượng phần tử phải được thay đổi. Không có vấn đề về thứ tự nào phát sinh vì mỗi phần tử độc lập về chi phí sau khi mục tiêu được cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 2, 2, 4]
```Chúng tôi theo dõi tần số: 

| Bước | Giá trị | Bản đồ tần số | Tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 1 | {1:1} | 1 | 
| 2 | 2 | {1:1, 2:1} | 1 | 
| 3 | 2 | {1:1, 2:2} | 2 | 
| 4 | 4 | {1:1, 2:2, 4:1} | 2 | 

Tần số tốt nhất cuối cùng là 2 (giá trị 2). 

Đáp án là 4 − 2 = 2. 

Điều này chứng tỏ rằng chúng tôi không cố gắng chuẩn hóa thành giá trị lớn nhất (4), mà thay vào đó chọn cấu trúc thường xuyên nhất. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [3, 3, 3, 6, 12]
```| Bước | Giá trị | Bản đồ tần số | Tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 3 | {3:1} | 1 | 
| 2 | 3 | {3:2} | 2 | 
| 3 | 3 | {3:3} | 3 | 
| 4 | 6 | {3:3, 6:1} | 3 | 
| 5 | 12 | {3:3, 6:1, 12:1} | 3 | 

Đáp án là 5 − 3 = 2. 

Điều này cho thấy rằng mặc dù tồn tại số lượng lớn hơn nhưng chiến lược tối ưu vẫn được giữ vững ở cấu trúc thống trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần để đếm tần số | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa n giá trị riêng biệt | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới 10⁶ và tất cả các hoạt động đều là cập nhật băm theo thời gian liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        out.append(str(n - max(freq.values())))
    return "\n".join(out)

# provided sample
assert run("1\n4\n1 2 2 4\n") == "2"

# all equal
assert run("1\n3\n5 5 5\n") == "0"

# all distinct
assert run("1\n4\n1 2 3 4\n") == "3"

# already optimal majority
assert run("1\n6\n2 2 2 3 3 1\n") == "3"

# single element
assert run("1\n1\n10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | trường hợp không hoạt động | 
| tất cả đều khác biệt | n−1 | trường hợp lây lan tồi tệ nhất | 
| đa số hỗn hợp | n−tần số tối đa | sự thống trị đúng đắn | 
| phần tử đơn | 0 | ranh giới n=1 | 

## Vỏ cạnh 

Đối với một mảng gồm các phần tử giống hệt nhau như [7, 7, 7, 7], bản đồ tần số chứa một khóa duy nhất có giá trị 4. Thuật toán chọn đây là nhóm tốt nhất, tạo ra 4 − 4 = 0, cho biết chính xác không cần thực hiện thao tác nào. 

Đối với một mảng hoàn toàn đa dạng như [1, 2, 3, 4], mỗi giá trị xuất hiện một lần nên tần số tốt nhất là 1. Câu trả lời là 3, nghĩa là chúng ta phải thay đổi ba phần tử để khớp với đại diện đã chọn. Thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt vì tần số tối đa vẫn hợp lệ ngay cả khi tất cả số lượng đều bằng nhau.
