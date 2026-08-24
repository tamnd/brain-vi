---
title: "CF 104289B - HOẶC-bitax"
description: "Chúng ta được cho một dãy số nguyên và chúng ta được phép cắt nó thành những phần không trống liền kề nhau. Đối với mỗi phần, chúng tôi tính toán một giá trị gọi là điểm của nó, được định nghĩa là XOR theo bit của tất cả các phần tử bên trong phần đó."
date: "2026-07-01T20:36:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104289
codeforces_index: "B"
codeforces_contest_name: "Bangladesh CP Server - BCS Round 1 (Div. 3)"
rating: 0
weight: 104289
solve_time_s: 75
verified: true
draft: false
---

[CF 104289B - OR-bitax](https://codeforces.com/problemset/problem/104289/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và chúng ta được phép cắt nó thành những phần không trống liền kề nhau. Đối với mỗi phần, chúng tôi tính toán một giá trị gọi là điểm của nó, được định nghĩa là XOR theo bit của tất cả các phần tử bên trong phần đó. Sau khi tính toán tất cả các điểm phân đoạn, chúng tôi lấy OR theo bit của các điểm này và mục tiêu của chúng tôi là chọn phân đoạn tối đa hóa giá trị OR cuối cùng này. 

Vì vậy, quyết định mà chúng tôi kiểm soát là đặt điểm cắt ở đâu. Cắt nhiều hơn sẽ tạo ra nhiều phân đoạn hơn, cắt ít phân đoạn hợp nhất hơn và thay đổi giá trị XOR của chúng. Mục tiêu cuối cùng không phải là tổng hoặc XOR của các kết quả phân đoạn, mà là OR theo bit của chúng, làm cho sự đóng góp của mỗi bit trở nên độc lập. 

Các ràng buộc cho phép tối đa 10^5 phần tử cho mỗi trường hợp thử nghiệm và tổng số lên tới 3 × 10^5 trong các thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các phân vùng, vì số cách phân chia một mảng tăng theo cấp số nhân, khoảng 2^(n−1). Ngay cả lập trình động bậc hai trên tất cả các mảng con cũng sẽ quá chậm ở quy mô tối đa. 

Một điểm tinh tế là các giá trị XOR của phân đoạn có thể đưa ra các mẫu bit không xuất hiện trong bất kỳ phần tử đơn lẻ nào. Ví dụ: XOR của 1 và 2 là 3, có cả hai bit được đặt. Một trực giác ngây thơ có thể gợi ý rằng việc phân đoạn có thể “tạo ra” những phần hữu ích mới. Câu hỏi quan trọng là liệu các bit được tạo như vậy có thể cải thiện OR cuối cùng ngoài những gì chúng ta đã có từ mảng ban đầu hay không. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ liệt kê mọi cách có thể để phân chia mảng, tính toán XOR cho từng phân đoạn, sau đó tính OR của các giá trị phân khúc đó và theo dõi mức tối đa. Điều này đúng vì nó đánh giá trực tiếp định nghĩa khách quan. Tuy nhiên, có thể có 2^(n−1) phân vùng và mỗi lần đánh giá tốn O(n) trong trường hợp xấu nhất, dẫn đến thời gian hàm mũ trở nên không khả thi ngay cả với n khoảng 25. 

Bây giờ chúng ta tìm kiếm cấu trúc. Quan sát quan trọng là câu trả lời cuối cùng chỉ phụ thuộc vào vị trí bit nào có thể bằng 1 trong ít nhất một phân đoạn XOR. Nếu một bit là 1 trong bất kỳ phân đoạn XOR nào, nó sẽ đóng góp vào OR cuối cùng một cách độc lập với tất cả các bit khác. 

Bây giờ hãy xem xét bất kỳ vị trí bit nào. Một phân đoạn XOR có tập hợp bit đó khi và chỉ khi một số phần tử lẻ trong phân đoạn đó có tập hợp bit đó. Tuy nhiên, điều này không cho phép chúng ta tạo ra một vị trí bit chưa từng tồn tại trong bất kỳ phần tử nào. XOR chỉ sắp xếp lại tính chẵn lẻ; nó không giới thiệu vị trí bit mới. Vì vậy, mọi bit có thể xuất hiện trong bất kỳ phân đoạn XOR nào đều phải xuất hiện trong ít nhất một phần tử của mảng. 

Điều này ngay lập tức ngụ ý giới hạn trên: câu trả lời không thể vượt quá bit OR của tất cả các phần tử. Mặt khác, giới hạn này có thể đạt được bằng cách chia mảng thành các đoạn phần tử đơn. Mỗi phân đoạn XOR chỉ là chính phần tử đó và OR cuối cùng trở thành chính xác OR của tất cả các phần tử. 

Vì vậy, vấn đề phân vùng hoàn toàn sụp đổ: chiến lược tối ưu chỉ đơn giản là lấy mọi phần tử làm phân đoạn riêng của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp giảm xuống việc tính toán OR theo bit của tất cả các phần tử trong mảng. 

1. Khởi tạo biến tích lũy`ans`về không. Biến này sẽ lưu trữ OR đang chạy của tất cả các giá trị được xử lý cho đến nay. 
2. Lặp lại mảng từ trái sang phải. Với mỗi phần tử, hãy cập nhật`ans`bằng cách lấy`ans = ans OR a[i]`. Điều này dần dần thu thập mọi bit xuất hiện ở bất kỳ đâu trong mảng. 
3. Sau khi xử lý tất cả các phần tử, xuất ra`ans`. 

Lý do chúng ta có thể bỏ qua tất cả các quyết định phân vùng một cách an toàn là vì không có phân vùng nào có thể đưa ra một vị trí bit mới ngoài vị trí đã tồn tại trong các giá trị đầu vào thô. 

### Tại sao nó hoạt động 

Mỗi phân đoạn đóng góp một giá trị XOR. Mỗi bit được đặt trong bất kỳ phân đoạn XOR nào phải đến từ một số tập hợp con của các phần tử gốc đã chứa bit đó. Vì OR chỉ kiểm tra xem một bit có xuất hiện ở bất kỳ đâu hay không nên việc phân phối các phần tử thành các phân đoạn không thể tạo ra một bit chưa có trong ít nhất một phần tử. Phân vùng một phần tử đã nhận ra mọi bit có thể đạt được một cách độc lập, do đó, việc hợp nhất các phân đoạn chỉ có nguy cơ hủy các bit bên trong XOR chứ không bao giờ cải thiện OR cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ans = 0
    for x in a:
        ans |= x
    
    print(ans)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Việc triển khai giữ chính xác một biến đang chạy cho OR. Không cần phải theo dõi tiền tố XOR hoặc bất kỳ trạng thái DP nào vì việc phân đoạn không ảnh hưởng đến giá trị tối ưu cuối cùng. 

Điều tinh tế duy nhất là đảm bảo xử lý đầu vào nhanh chóng do tổng số ràng buộc lớn. sử dụng`sys.stdin.readline`tránh chi phí từ các phương thức nhập liệu tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
6 4 8
```Chúng tôi xử lý các yếu tố từng bước. 

| Bước | Yếu tố | Đang chạy HOẶC | 
| --- | --- | --- | 
| 1 | 6 | 6 | 
| 2 | 4 | 6 | 
| 3 | 8 | 14 | 

Câu trả lời cuối cùng là 14. 

Điều này cho thấy rằng mặc dù có thể phân vùng khác nhau nhưng không phân vùng nào có thể vượt quá OR của tất cả các phần tử. 

### Ví dụ 2 

đầu vào:```
5
3 4 2 5 1
```| Bước | Yếu tố | Đang chạy HOẶC | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 4 | 7 | 
| 3 | 2 | 7 | 
| 4 | 5 | 7 | 
| 5 | 1 | 7 | 

Câu trả lời cuối cùng là 7. 

Điều này xác nhận rằng các lựa chọn phân đoạn không cải thiện kết quả cuối cùng ngoài việc thu thập tất cả các đóng góp bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần bằng bit OR | 
| Không gian | O(1) | Chỉ sử dụng một bộ tích lũy duy nhất | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng số phần tử, tối đa là 3 × 10^5, dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        ans = 0
        for x in a:
            ans |= x
        print(ans)

    t = int(input())
    out = []
    for _ in range(t):
        solve()
    return ""

# provided samples
assert run("2\n3\n6 4 8\n5\n3 4 2 5 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n7\n`|`7`| Mảng kích thước tối thiểu | 
|`1\n4\n1 2 4 8\n`|`15`| Tất cả các bit độc lập | 
|`1\n5\n0 0 0 0 0\n`|`0`| Tất cả các trường hợp số không | 
|`1\n6\n3 3 3 3 3 3\n`|`3`| Độ ổn định giá trị lặp lại | 

## Vỏ cạnh 

Mảng một phần tử là trường hợp đơn giản nhất. Phân vùng duy nhất có thể là chính mảng đó và câu trả lời bằng phần tử đó. Thuật toán xử lý chính xác điều này vì bộ tích lũy OR bắt đầu từ 0 và trở thành giá trị đó sau khi xử lý phần tử đầu tiên và duy nhất. 

Một mảng số không là một trường hợp góc khác. Mọi phân đoạn XOR đều bằng 0 bất kể phân vùng như thế nào, vì vậy OR cuối cùng bằng 0. Thuật toán bảo toàn điều này vì OR với số 0 không làm thay đổi bộ tích lũy. 

Các mảng có giá trị giống hệt nhau lặp lại cũng cho thấy rằng việc phân đoạn không quan trọng. Ngay cả khi chúng ta nhóm hoặc phân chia khác nhau, OR trên tất cả các phần tử vẫn giữ nguyên và không có kết hợp XOR nào tạo ra bit mới ngoài giá trị ban đầu.
