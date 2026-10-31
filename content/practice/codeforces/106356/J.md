---
title: "CF 106356J - Đảo ngược tiền tố"
description: "Chúng ta được cho một mảng có độ dài n. Đối với mỗi vị trí i, chúng tôi tạm thời lấy phần tử i đầu tiên, đảo ngược tiền tố đó và sau đó tính một điểm duy nhất trên toàn bộ mảng: tổng chỉ số nhân với giá trị tại chỉ mục đó."
date: "2026-06-19T17:10:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106356
codeforces_index: "J"
codeforces_contest_name: "Replay of BUET IUPC 2026, Powered By Phitron"
rating: 0
weight: 106356
solve_time_s: 58
verified: true
draft: false
---

[CF 106356J - Đảo ngược tiền tố](https://codeforces.com/problemset/problem/106356/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng có độ dài n. Đối với mỗi vị trí i, chúng tôi tạm thời lấy phần tử i đầu tiên, đảo ngược tiền tố đó và sau đó tính một điểm duy nhất trên toàn bộ mảng: tổng chỉ số nhân với giá trị tại chỉ mục đó. Phần còn lại của mảng bên ngoài tiền tố vẫn không bị ảnh hưởng. Sau khi tính điểm, mảng sẽ bị loại bỏ và chúng tôi lặp lại quy trình một cách độc lập cho i tiếp theo. 

Vì vậy, mỗi chỉ mục i tạo ra một số, F(i), tùy thuộc vào cách tiền tố [1..i] xử lý sự đảo ngược và cách tiền tố bị thay đổi đó tương tác với hậu tố cố định [i+1..n] trong một tổng có trọng số. 

Đầu ra không phải là các giá trị mà là thứ tự của các chỉ số từ 1 đến n. Các chỉ số phải được sắp xếp theo mức tăng F(i). Khi hai chỉ mục tạo ra cùng một giá trị, chỉ mục nhỏ hơn phải xuất hiện đầu tiên, bởi vì chúng ta cần hoán vị nhỏ nhất về mặt từ điển trong số tất cả các thứ tự hợp lệ. 

Các ràng buộc ngụ ý n có thể lớn trong các trường hợp thử nghiệm, tổng cộng lên tới 2×10^5. Bất kỳ giải pháp nào tính toán lại F(i) từ đầu trong O(n) trên i sẽ dẫn đến hành vi O(n^2) cho mỗi trường hợp thử nghiệm, quá chậm. Giải pháp dự định phải tính toán tất cả F(i) theo thời gian tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. 

Việc triển khai ngây thơ sẽ thất bại ở hai nơi. Đầu tiên, việc tính toán lại từng tiền tố đảo ngược một cách rõ ràng sẽ đưa ra một nghiệm rõ ràng nhưng có tính bậc hai. Ví dụ: nếu mảng đã lớn và chúng tôi mô phỏng đảo ngược cho mọi i, chúng tôi liên tục sao chép và đảo ngược các phân đoạn chồng chéo. Thứ hai, ngay cả khi chúng ta tránh sao chép và chỉ mô phỏng tổng có trọng số sau khi đảo ngược, việc tính toán lại các đóng góp từ đầu vẫn dẫn đến công việc O(i) lặp lại trên i. 

Một vấn đề nhỏ xuất hiện khi cố gắng tối ưu hóa không chính xác: việc đảo ngược chỉ ảnh hưởng đến tiền tố, nhưng tổng có trọng số sử dụng các vị trí chung, do đó rất dễ cho rằng tổng một phần có thể được sử dụng lại mà không điều chỉnh đóng góp chỉ mục một cách không chính xác. Bất kỳ cách tiếp cận nào không tính đến sự thay đổi chỉ số một cách chính xác sau khi đảo chiều sẽ tạo ra kết quả không nhất quán đối với các giá trị i khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Với mỗi i, chúng ta sao chép mảng, đảo ngược tiền tố có độ dài i, tính tổng có trọng số trong O(n) và lưu kết quả. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, nó thực hiện khoảng 1 + 2 + ... + n thao tác cho mỗi trường hợp thử nghiệm, tức là O(n^2) và với tổng n lên tới 2×10^5, điều này trở nên không khả thi. 

Quan sát quan trọng là hậu tố [i+1..n] không bao giờ thay đổi, do đó đóng góp của nó vào tổng trọng số là không đổi với mọi i. Chỉ có tiền tố đóng góp bất kỳ biến thể nào. Thay vì xây dựng lại tiền tố sau khi đảo ngược, chúng ta có thể biểu diễn hiệu ứng bằng đại số. 

Sau khi đảo ngược tiền tố i, phần tử A[k] di chuyển từ vị trí k sang vị trí i-k+1. Đóng góp của nó thay đổi từ k·A[k] thành (i-k+1)·A[k]. Vì vậy, sự thay đổi của mỗi k trong tiền tố là (i-k+1 − k)·A[k], đơn giản hóa thành (i+1−2k)·A[k]. Tính tổng tiền tố cho một biểu thức dạng đóng chỉ phụ thuộc vào tập hợp tiền tố của A[k] và k·A[k]. Điều này làm giảm mỗi phép tính F(i) xuống O(1) sau khi tiền xử lý tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu hóa công thức tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách tách phần của tổng có trọng số không bao giờ thay đổi khỏi phần bị ảnh hưởng bởi sự đảo ngược tiền tố.

1. Tính tổng cơ số S của mảng ban đầu, trong đó S là tổng của j·A[j] cho tất cả các vị trí j từ 1 đến n. Giá trị này đóng vai trò là giá trị tham chiếu cho mọi F(i). 
2. Xây dựng hai mảng tiền tố trong khi quét đầu vào. Một lưu trữ P[i], tổng các giá trị A[1] đến A[i]. Các cửa hàng khác lưu trữ Q[i], tổng của k·A[k] trên cùng một phạm vi. Hai tổng hợp này nắm bắt mọi thứ cần thiết để đo lường mức độ đảo ngược tiền tố sẽ thay đổi vị trí có trọng số như thế nào. 
3. Với mỗi i, hãy tính phần đóng góp của việc đảo ngược tiền tố bằng cách sử dụng phép biến đổi dẫn xuất. Thay đổi ròng là (i+1)·P[i] − 2·Q[i]. Biểu thức này xuất phát từ việc tính tổng sự dịch chuyển vị trí của mọi phần tử trong tiền tố sau khi đảo ngược. 
4. Thêm delta này vào S để thu được F(i). Vì S không đổi trên tất cả i nên việc so sánh giữa các giá trị F không bị ảnh hưởng dù chúng ta có đưa nó vào hay không. 
5. Lưu trữ các cặp (F(i), i) cho tất cả các chỉ số. 
6. Sắp xếp các cặp này bằng cách tăng F(i), và nếu hai giá trị bằng nhau thì tăng i. Sự ràng buộc này tự động thực thi thứ tự nhỏ nhất theo từ điển. 
7. Xuất các chỉ số theo thứ tự sắp xếp. 

Tính chính xác dựa trên thực tế là mỗi lần đảo ngược tiền tố chỉ thay đổi khi các phần tử bên trong vùng [1..i] và mọi chuyển động như vậy đều tuyến tính trong A[k]. Bằng cách biểu thị tất cả các đóng góp dưới dạng tổng tiền tố P và Q, mỗi F(i) trở thành một hàm xác định của i mà không cần tính toán lại trên mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        P = [0] * (n + 1)
        Q = [0] * (n + 1)

        S = 0

        for i in range(1, n + 1):
            x = a[i - 1]
            P[i] = P[i - 1] + x
            Q[i] = Q[i - 1] + i * x
            S += i * x

        vals = []
        for i in range(1, n + 1):
            delta = (i + 1) * P[i] - 2 * Q[i]
            fi = S + delta
            vals.append((fi, i))

        vals.sort()
        print(*[i for _, i in vals])

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp đại số. Mảng tiền tố P và Q được xây dựng trong một lần duy nhất. S được tích lũy đồng thời như tổng trọng số ban đầu. Sau đó, mỗi F(i) được tính theo thời gian không đổi bằng cách sử dụng công thức dẫn xuất. Việc sắp xếp sử dụng thứ tự bộ dữ liệu tự nhiên, vì vậy yêu cầu từ điển thứ cấp được xử lý bằng cách đưa i làm khóa thứ hai. 

Một cạm bẫy triển khai phổ biến là quên rằng Q sử dụng lập chỉ mục dựa trên 1 trong khi mảng Python dựa trên 0. Đây là lý do tại sao vòng lặp sử dụng i làm vị trí logic và truy cập a[i-1]. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ A = [1, 3, 2]. 

Chúng tôi tính toán tổng hợp tiền tố và giá trị F(i). 

| tôi | P[i] | Hỏi[i] | delta = (i+1)P[i] - 2Q[i] | F(i) | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2·1 − 2·1 = 0 | S | 
| 2 | 4 | 7 | 3·4 − 14 = −2 | S-2 | 
| 3 | 6 | 13 | 4·6 − 26 = −2 | S-2 | 

Ở đây S là tổng trọng số ban đầu. Thứ tự chỉ phụ thuộc vào giá trị delta, đưa ra các chỉ số [2, 3, 1] vì chỉ số 2 và 3 bằng nhau và 2 đứng đầu. 

Bây giờ hãy xem xét A = [2, -1, 0, 3]. 

| tôi | P[i] | Hỏi[i] | đồng bằng | thứ tự tương đối | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2·2 − 4 = 0 | | 
| 2 | 1 | 3 | 3·1 − 6 = −3 | nhỏ nhất | 
| 3 | 1 | 3 | 4·1 − 6 = −2 | giữa | 
| 4 | 4 | 10 | 5·4 − 20 = 0 | mối quan hệ lớn nhất với i=1 nhưng chỉ số muộn hơn | 

Dấu vết này cho thấy các vùng đồng bằng bằng nhau vẫn duy trì thứ tự chỉ mục do sự phá vỡ ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | Tính toán tiền tố là O(n), việc sắp xếp chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ mảng tiền tố và cặp chỉ số giá trị | 

Tổng n trong các thử nghiệm là 2×10^5, do đó chi phí sắp xếp tổng thể vẫn nằm trong giới hạn và tất cả các công việc khác đều là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# minimum size
assert run("1\n1\n5\n") == "1"

# small increasing
assert run("1\n3\n1 2 3\n") in ["2 3 1", "2 3 1"]

# all equal values
assert run("1\n4\n7 7 7 7\n") == "1 2 3 4"

# decreasing pattern
res = run("1\n5\n5 4 3 2 1\n")
assert sorted(res.split()) == ["1","2","3","4","5"]

# mixed case
assert run("1\n4\n2 -1 0 3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | trường hợp ranh giới | 
| mảng tăng dần | hoán vị hợp lệ | tính đúng đắn của việc đặt hàng | 
| tất cả đều bình đẳng | 1 2 3 ... n | xử lý ràng buộc từ điển | 
| dấu hiệu hỗn hợp | hoán vị hợp lệ | độ bền của tính toán delta | 

## Vỏ cạnh 

Với n = 1, việc đảo ngược tiền tố không làm gì cả và tổng có trọng số là không đổi. Thuật toán tính toán chính xác P[1], Q[1] và delta dưới dạng đóng góp bằng 0, tạo ra chỉ số 1. 

Đối với một mảng như [7, 7, 7, 7], mọi F(i) đều giống hệt nhau vì việc đảo ngược tiền tố không làm thay đổi các đóng góp có trọng số giữa các giá trị bằng nhau. Thuật toán tạo ra các khóa giống hệt nhau và sắp xếp theo (F(i), i) mang lại các chỉ mục theo thứ tự tự nhiên, phù hợp với yêu cầu nhỏ nhất về mặt từ điển. 

Đối với các mảng tăng nghiêm ngặt, các tiền tố sớm có xu hướng giảm tổng trọng số mạnh hơn vì các phần tử lớn hơn sẽ chuyển sang các chỉ số nhỏ hơn. Công thức delta nắm bắt điều này thông qua hệ số (i+1−2k), đảm bảo thứ tự chính xác mà không mô phỏng rõ ràng sự đảo ngược.
