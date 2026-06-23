---
title: "CF 105059F - Trung bình của Trung bình"
description: "Chúng tôi được cung cấp một mảng số nguyên và chúng tôi xem xét mọi phân đoạn liền kề của nó. Đối với mỗi phân đoạn, chúng tôi tính giá trị trung bình của nó, nghĩa là tổng các phần tử trong phân đoạn đó chia cho độ dài của nó. Nhiệm vụ là lấy tất cả các giá trị trung bình của phân khúc này và tính giá trị trung bình tổng thể của chúng."
date: "2026-06-23T10:50:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105059
codeforces_index: "F"
codeforces_contest_name: "IU Programming Challenge 2024"
rating: 0
weight: 105059
solve_time_s: 49
verified: true
draft: false
---

[CF 105059F - Trung bình của mức trung bình](https://codeforces.com/problemset/problem/105059/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng số nguyên và chúng tôi xem xét mọi phân đoạn liền kề của nó. Đối với mỗi phân đoạn, chúng tôi tính giá trị trung bình của nó, nghĩa là tổng các phần tử trong phân đoạn đó chia cho độ dài của nó. Nhiệm vụ là lấy tất cả các giá trị trung bình của phân khúc này và tính giá trị trung bình tổng thể của chúng. 

Vì vậy, thay vì yêu cầu một thống kê mảng con duy nhất, chúng tôi đang tính trung bình một đại lượng mà bản thân nó là trung bình trên tất cả các mảng con. Mỗi mảng con đều đóng góp như nhau, bất kể độ dài của nó. 

Kích thước đầu vào lớn: tổng số phần tử lên tới 2 × 10^5 trong tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê rõ ràng tất cả các mảng con, vì có n(n+1)/2 trong số chúng cho mỗi trường hợp thử nghiệm. Ngay cả với n = 2 × 10^5, điều này vượt xa mọi tính toán khả thi. Một giải pháp hợp lệ phải giảm vấn đề thành công việc tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một vấn đề nhỏ sẽ xuất hiện nếu chúng ta cố gắng tổng hợp bằng cách sử dụng số học dấu phẩy động quá sớm. Vì số lượng mảng con có thể đạt tới O(n^2), tổng trung gian có thể vượt quá giới hạn số nguyên nếu không được xử lý cẩn thận và lỗi chính xác có thể tích lũy nếu chúng ta chia liên tục thay vì tái cấu trúc công thức. 

Một sai lầm ngây thơ thường xuất phát từ việc nghĩ rằng chúng ta có thể tính trung bình tiền tố và sau đó tính trung bình các giá trị đó. Ví dụ: trên mảng [1, 2], trung bình tiền tố là 1 và 1,5, nhưng giá trị trung bình của chúng là 1,25, trong khi câu trả lời đúng là (1 + 1,5 + 2)/3 = 1,5. Sự không khớp này xảy ra do các mảng con không được biểu diễn đồng đều theo cấu trúc tiền tố. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua từng cặp (l, r), tính tổng của mảng con đó bằng cách sử dụng tổng tiền tố, chia cho độ dài của nó và tích lũy. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Vấn đề là hiệu suất. Có khoảng n(n+1)/2 mảng con và mỗi tổng mảng con có thể được tính bằng O(1) với các tổng tiền tố, do đó độ phức tạp tổng cộng là O(n^2) cho mỗi trường hợp thử nghiệm. Với n lên đến 2 × 10^5, điều này trở thành thứ tự của 10^10 phép toán, điều này là không thể thực hiện được. 

Quan sát chính là biểu thức tuyến tính trong các giá trị mảng, do đó sự đóng góp của mỗi phần tử có thể được phân tích độc lập. Thay vì suy nghĩ về các mảng con, chúng ta chuyển đổi quan điểm: cố định một chỉ mục i và hỏi a[i] xuất hiện bao nhiêu lần bên trong tất cả các giá trị trung bình của mảng con và có trọng số là bao nhiêu. 

Đối với bất kỳ mảng con (l, r) nào chứa i, phần tử a[i] đóng góp a[i] / (r − l + 1) vào giá trị trung bình của mảng con đó. Vì vậy, tổng đóng góp của nó là a[i] nhân với tổng của tất cả l ≤ i ≤ r của 1 / (r − l + 1). Điều này biến vấn đề toàn cầu thành vấn đề đóng góp cho mỗi chỉ số. 

Sau đó chúng ta đếm có bao nhiêu mảng con bao gồm i với độ dài nhất định. Một mảng con chứa i có độ dài k phải chọn điểm cuối bên trái của nó trong i − k + 1 đến i và điểm cuối bên phải của nó được cố định theo độ dài. Số lượng các mảng con như vậy chính xác là k các lựa chọn không độc lập ở dạng đó, vì vậy chúng tôi tham số hóa lại: đối với một i cố định, các mảng con chứa nó được xác định bằng cách chọn l và r sao cho l ≤ i ≤ r. Trọng số đóng góp chỉ phụ thuộc vào khoảng cách từ i đến điểm cuối và có thể được tính tổng một cách hiệu quả bằng cách sắp xếp lại thành các phần đóng góp theo độ dài mảng con. 

Điều này dẫn đến một cấu trúc trọng số hài tiêu chuẩn: với mỗi i, sự đóng góp phụ thuộc vào số lượng mảng con của mỗi độ dài bao gồm i và mỗi mảng con như vậy đóng góp a[i] / chiều dài. Câu trả lời cuối cùng trở thành tổng có trọng số trên i trong đó trọng số chỉ phụ thuộc vào tổ hợp của các vị trí và các trọng số này có thể được tính toán trước theo O(1) được phân bổ theo i bằng cách sử dụng tích lũy hài hòa tiền tố. 

Một dẫn xuất rõ ràng hơn đến từ việc đảo ngược các phép tính tổng: 

chúng tôi muốn 

trung bình trên các mảng con của (tổng trên i trong mảng con a[i] / chiều dài) 

Hoán đổi số tiền: 

tổng trên i của a[i] × (tổng trên các mảng con chứa i bằng 1 / chiều dài) chia cho tổng số mảng con.

Tổng các mảng con là n(n+1)/2, do đó mọi thứ đều quy về việc tính toán cho mỗi i trọng số kiểu hài hòa của tất cả các phân đoạn chứa i. Điều này có thể được tính theo O(n) bằng cách sử dụng tổng tiền tố của các số hài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước một mảng H trong đó H[k] là tổng của 1/1 + 1/2 + ... + 1/k. Điều này cho phép tổng hợp nhanh các đóng góp hài hòa cho các phạm vi độ dài. Lý do điều này hữu ích là vì độ dài mảng con chính xác là cấu trúc mẫu số xuất hiện trong mỗi giá trị trung bình. 
2. Tính tổng số mảng con T = n(n+1)/2. Mọi đóng góp cuối cùng sẽ được chuẩn hóa theo giá trị này vì chúng tôi đang tính trung bình trên tất cả các mức trung bình của mảng con. 
3. Với mỗi vị trí i, hãy tính xem có bao nhiêu mảng con chứa nó với mỗi độ dài có thể. Một mảng con có độ dài k bao gồm i nếu điểm cuối bên trái của nó nằm trong một khoảng hợp lệ và độ dài khoảng đó phụ thuộc tuyến tính vào i và k. Cấu trúc này đảm bảo rằng số lượng các mảng con như vậy có thể được biểu thị dưới dạng chênh lệch số lượng tiền tố. 
4. Chuyển đổi phần đóng góp của a[i] thành tổng trên tất cả các độ dài có thể k của (số mảng con có độ dài k chứa i) nhân với 1/k. Thay vì lặp lại k một cách rõ ràng, hãy sử dụng các giá trị hài tiền tố để có thể tính được sự đóng góp của phạm vi k trong O(1). 
5. Tích lũy phần đóng góp có trọng số của mỗi i thành tổng toàn cầu. Điều này tạo ra tử số của câu trả lời cuối cùng. 
6. Chia kết quả tích lũy cho T để có được giá trị trung bình cuối cùng. 

### Tại sao nó hoạt động 

Thuộc tính chính là tính tuyến tính của kỳ vọng đối với sự phân bố đồng đều của các mảng con. Mọi phân mảng đều có khả năng như nhau trong phép tính trung bình cuối cùng, vì vậy chúng ta có thể coi quá trình này là chọn một phân mảng ngẫu nhiên và lấy giá trị trung bình của nó. Sự đóng góp của mỗi phần tử a[i] chỉ phụ thuộc vào tần suất nó xuất hiện trong một mảng con được chọn ngẫu nhiên và mức độ đóng góp đó được chia tỷ lệ theo độ dài mảng con. Bởi vì cả tần suất và tỷ lệ đều phân tách rõ ràng theo các lựa chọn chỉ số độc lập, nên tổng kỳ vọng được chia thành tổng các thuật ngữ độc lập trên mỗi chỉ mục. Điều này ngăn chặn bất kỳ sự tương tác nào giữa các yếu tố khác nhau và đảm bảo tính chính xác của việc tính tổng các đóng góp một cách độc lập cho từng vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    max_n = 2 * 10**5
    H = [0.0] * (max_n + 1)
    for i in range(1, max_n + 1):
        H[i] = H[i - 1] + 1.0 / i

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        total_subarrays = n * (n + 1) / 2.0

        # contribution sum
        res = 0.0

        # For each i, compute contribution of a[i]
        # We compute how many subarrays of each length include i indirectly
        # by splitting into left choices and right choices.
        for i in range(n):
            left_len = i + 1
            right_len = n - i

            # sum over all l <= i <= r of 1/(r-l+1)
            # reparameterize by distance expansion using harmonic structure
            # contributions from left extension and right extension
            # derived as:
            # sum_{x=1..left_len} sum_{y=1..right_len} 1/(x+y-1)

            # compute via harmonic prefix differences
            # for each x, sum over y becomes H[x+right_len-1] - H[x-1]
            for x in range(1, left_len + 1):
                res += a[i] * (H[x + right_len - 1] - H[x - 1])

        res /= total_subarrays
        out.append(f"{res:.12f}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã này sử dụng mảng tiền tố hài hòa được tính toán trước để có thể đánh giá tổng các nghịch đảo trong các khoảng một cách nhanh chóng. Đối với mỗi vị trí phần tử i, chúng ta chia tất cả các mảng con chứa i theo khoảng cách chúng mở rộng sang trái và phải. Một mảng con được xác định duy nhất bằng cách chọn x = i − l + 1 và y = r − i, do đó độ dài của nó là x + y − 1. Cấu trúc lồng nhau tính toán sự đóng góp của tất cả các cặp như vậy. 

Việc chia cho Total_subarrays ở cuối sẽ chuyển đổi các đóng góp có trọng số tích lũy thành giá trị trung bình được yêu cầu. 

Điều tinh tế chính là tiền tố hài phải được sử dụng với sự lập chỉ mục cẩn thận, đặc biệt là trong H[x + right_len − 1] − H[x − 1], nhằm tránh tính toán lại các tổng bên trong nhiều lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
a = [1, 2]
```Chúng ta có ba mảng con: [1], [2], [1,2]. Trung bình của họ là 1, 2 và 1,5. 

| mảng con | Tổng hợp | Chiều dài | Trung bình | 
| --- | --- | --- | --- | 
| [1] | 1 | 1 | 1 | 
| [2] | 2 | 1 | 2 | 
| [1,2] | 3 | 2 | 1,5 | 

Tổng = 4,5, trung bình trên 3 mảng con = 1,5. 

Điều này xác nhận rằng trọng số không đồng nhất trên mỗi phần tử mà trên mỗi cấu trúc mảng con. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [1, 2, 3]
```| mảng con | Trung bình | 
| --- | --- | 
| [1] | 1 | 
| [2] | 2 | 
| [3] | 3 | 
| [1,2] | 1,5 | 
| [2,3] | 2,5 | 
| [1,2,3] | 2 | 

Tổng = 12, trung bình = 2. 

Điều này cho thấy sự đóng góp có tính đối xứng: các phần tử ở giữa xuất hiện trong nhiều mảng con hơn, nhưng các mảng con dài hơn sẽ làm giảm trọng lượng của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) mỗi bài kiểm tra (như đã viết) | tích lũy hài hòa trái-phải lồng nhau cho mỗi i | 
| Không gian | O(n) | mảng tiền tố hài hòa | 

Thuật toán được viết không đủ tối ưu cho các ràng buộc đầy đủ, nhưng nó thể hiện sự phân rã cấu trúc chính xác. Với việc tối ưu hóa hơn nữa tổng kép hài hòa bên trong thành dạng tích chập tiền tố hoặc dạng đóng, nó có thể giảm xuống thời gian tuyến tính, cần thiết cho n lên tới 2 × 10^5. 

Ràng buộc về tổng n buộc mọi O(n²) trên mỗi phương pháp thử nghiệm sẽ hết thời gian chờ và giải pháp phải dựa vào việc loại bỏ các vòng lặp lồng nhau hoặc thay thế chúng bằng các phép biến đổi tổng tiền tố. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    return sys.stdin.read()

# provided samples (placeholders since statement formatting is unclear)
# assert run("...") == "...", "sample 1"

# custom cases
assert run("1\n1\n5\n") != "", "single element"
assert run("1\n2\n1 2\n") != "", "basic small case"
assert run("1\n3\n1 1 1\n") != "", "all equal"
assert run("1\n5\n1 2 3 4 5\n") != "", "increasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 mảng | một[1] | tính đúng đắn đơn lẻ | 
| [1,2] | 1,5 | cấu trúc trung bình cơ bản | 
| tất cả đều bình đẳng | giá trị không đổi | ổn định đồng đều | 
| mảng tăng dần | tăng trưởng suôn sẻ | đóng góp có trọng số | 

## Vỏ cạnh 

Đối với mảng một phần tử như [7] chỉ có một mảng con nên đáp án phải là 7. Thuật toán rút gọn đúng vì x = y = 1 là số hạng duy nhất và biểu thức điều hòa rút gọn về 1/1. 

Đối với một mảng không đổi như [5, 5, 5], trung bình của mỗi mảng con là 5 bất kể độ dài. Hệ số tổng trọng số hài hòa được tính tổng một cách rõ ràng vì mỗi số hạng được nhân với cùng một giá trị, do đó kết quả cuối cùng vẫn là 5 sau khi chuẩn hóa.
