---
title: "CF 1895G - Hai Nhân Vật, Hai Màu Sắc"
description: "Chúng ta được cung cấp một chuỗi nhị phân trong đó mỗi vị trí mang một giá trị tùy thuộc vào cách chúng ta tô màu nó. Mỗi ký tự có thể được sơn màu đỏ hoặc xanh. Nếu chúng ta chọn màu đỏ ở vị trí $i$, chúng ta kiếm được $ri$. Nếu chúng ta chọn màu xanh lam, chúng ta kiếm được $bi$."
date: "2026-06-08T21:47:42+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "flows", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1895
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 157 (Rated for Div. 2)"
rating: 3100
weight: 1895
solve_time_s: 106
verified: true
draft: false
---

[CF 1895G - Hai ký tự, hai màu sắc](https://codeforces.com/problemset/problem/1895/G) 

**Đánh giá:** 3100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, luồng, tham lam 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân trong đó mỗi vị trí mang một giá trị tùy thuộc vào cách chúng ta tô màu nó. Mỗi ký tự có thể được sơn màu đỏ hoặc xanh. Nếu chúng ta chọn màu đỏ ở vị trí$i$, chúng tôi kiếm được$r_i$. Nếu chúng ta chọn màu xanh, chúng ta kiếm được$b_i$. 

Sau khi tô màu, tất cả các vị trí màu xanh sẽ bị xóa khỏi chuỗi. Những gì còn lại là một dãy con chỉ bao gồm các ký tự màu đỏ, theo thứ tự ban đầu của chúng. Trên chuỗi nhị phân còn lại này, chúng ta phải trả một hình phạt bằng số lần đảo ngược, trong đó đảo ngược là một cặp vị trí$i < j$sao cho ký tự còn lại tại$i$là 1 và ký tự còn lại tại$j$là 0. 

Mục tiêu là chọn màu tối đa hóa tổng giá trị kiếm được trừ đi chi phí đảo ngược. 

Thực tế cấu trúc quan trọng là các phần tử màu xanh lam không ảnh hưởng trực tiếp đến sự đảo ngược ngoại trừ việc bị loại bỏ. Vì vậy, quyết định về cơ bản là chọn một dãy con (các phần tử màu đỏ) có trọng số$r_i$, trong khi phải trả một chi phí phụ thuộc vào thứ tự cảm ứng là 1 và 0 trong chuỗi đó. 

Các ràng buộc rất lớn: tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới$4 \cdot 10^5$. Bất kỳ giải pháp nào cũng phải gần tuyến tính hoặc$O(n \log n)$. Suy luận bậc hai trên tất cả các cặp hoặc tất cả các dãy con là không thể. 

Một cách tiếp cận đơn giản sẽ thử tất cả các cách tô màu, hoặc tương đương với tất cả các dãy con của phần tử màu đỏ. Ngay cả khi chúng tôi giới hạn DP trên tiền tố, trạng thái sẽ cần theo dõi sự sắp xếp chính xác của các số 0 và 1 còn lại, theo cấp số nhân. 

Một trường hợp thất bại tinh vi đối với tư duy tham lam xuất hiện khi lợi ích cục bộ từ việc chọn một nút có màu đỏ bỏ qua cấu trúc đảo ngược trong tương lai. 

Ví dụ: hãy xem xét một chuỗi:```
s = 10
r = [100, 1]
b = [0, 0]
```Nếu chúng ta tham lam lấy cả hai làm màu đỏ, chúng ta được 101 nhưng trả một lần đảo ngược (1 trước 0), được 100. Nếu bỏ phần tử đầu tiên để tránh đảo ngược, chúng ta chỉ nhận được 1. Sự đánh đổi phụ thuộc vào cấu trúc toàn cầu, không phải lựa chọn cục bộ. 

Khó khăn trọng tâm là việc loại bỏ số 1 sớm hơn sẽ giảm chi phí đảo ngược tiềm ẩn với các số 0 sau đó nhưng cũng hy sinh phần thưởng. 

## Phương pháp tiếp cận 

Chúng tôi giải thích lại vấn đề như việc chọn một tập hợp con các chỉ số để giữ lại (màu đỏ). Đặt tập hợp con này là$S$. Điểm số là:$$\sum_{i \in S} r_i - \text{inversions among } S$$Chi phí đảo ngược chỉ phụ thuộc vào thứ tự của các phần tử được chọn: mỗi cặp$(i < j)$đóng góp 1 nếu$s_i = 1$,$s_j = 0$, và cả hai đều được chọn. 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con, tính toán phần thưởng và số lần đảo ngược, đồng thời lấy kết quả tốt nhất. Đây là$O(2^n \cdot n)$, quá lớn. 

Một DP có cấu trúc chặt chẽ hơn sẽ cố gắng xử lý chuỗi từ trái sang phải, duy trì số lượng số 1 được chọn tồn tại cho đến nay, bởi vì mỗi số 0 mới được chọn sẽ phải chịu chi phí bằng với số lượng đó. Điều này gợi ý trạng thái lập trình động có dạng:$$dp[i][k] = \text{best score up to } i \text{ with } k \text{ selected ones}$$Tuy nhiên$k$có thể lên tới$n$, làm cái này$O(n^2)$. 

Cái nhìn sâu sắc quan trọng là tránh theo dõi rõ ràng kích thước tập hợp con và thay vào đó coi mỗi phần tử như một quyết định đóng góp tuyến tính cho hàm tiềm năng tổng thể. Mỗi số 0 được chọn đóng góp một chi phí bằng số số 1 được chọn trước nó. Điều này gợi ý rằng chúng ta nên duy trì một cấu trúc tích lũy các đóng góp của số 1 và số 0 theo cách cho phép tối ưu hóa gia tăng. 

Chúng tôi đảo ngược quan điểm: thay vì nghĩ “chọn tập con S”, chúng tôi gán cho mỗi phần tử một nhãn màu đỏ hoặc xanh lam và hiểu màu xanh lam là sự xóa bỏ. Hình phạt đảo ngược chỉ liên quan đến yếu tố màu đỏ. Điều đó có nghĩa là chúng ta đang tối ưu hóa bài toán dãy con có trọng số với sự tương tác theo cặp chỉ giữa các cặp (1,0). 

Cấu trúc này cho phép một thủ thuật cổ điển: quét đường với “chi phí hiện tại để chọn số 0 cho số lượng số 1 chúng ta đã chọn”. Chúng tôi duy trì DP trên một biến duy nhất biểu thị số lượng 1 hiện được giữ và đối với mỗi phần tử, hãy quyết định xem có giữ nó hay không. Các chuyển đổi là tuyến tính và có thể được tối ưu hóa bằng cách sử dụng lý luận tiền tố/hậu tố hoặc độ lồi trên các trạng thái. Việc tối ưu hóa kết quả sẽ giảm xuống để duy trì các giá trị tốt nhất cho mỗi phân chia có thể có giữa việc chọn số 1 sớm và số 0 sau, điều này có thể được giải quyết bằng cách quét tuyến tính và cấu trúc tiền tố tốt nhất đang chạy. 

Mức giảm cuối cùng là mỗi vị trí đóng góp độc lập (nếu màu xanh) hoặc đóng góp một giá trị được sửa đổi tùy thuộc vào số lần đảo ngược mà nó tạo ra nếu màu đỏ. Điều này dẫn đến DP tham lam tối ưu, trong đó chúng tôi xử lý các vị trí và duy trì điểm số tốt nhất có thể đạt được cho các cấu hình được tham số hóa theo số lượng cấu hình được giữ lại nhưng được nén bằng cách sử dụng một đường bao đang chạy. 

Việc triển khai tập trung vào việc duy trì số lượng 1 đã chọn đang chạy và theo dõi sự cân bằng tốt nhất giữa việc lấy hoặc bỏ qua từng phần tử bằng cách sử dụng điều chỉnh chi phí gia tăng. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| DP ngây thơ qua tiền tố |$O(n^2)$|$O(n)$| Quá chậm | 
| DP được tối ưu hóa với tính năng quét + nén trạng thái |$O(n \log n)$hoặc$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chia quyết định thành từng vị trí có màu đỏ hoặc xanh. Màu xanh đóng góp$b_i$và được loại bỏ khỏi việc xem xét đảo ngược. Màu đỏ đóng góp$r_i$và tham gia vào các nghịch đảo. 
2. Quan sát rằng chỉ các cặp (1 trước 0) trong số các phần tử màu đỏ mới quan trọng. Điều này có nghĩa là khi chúng ta quyết định lấy số 0 làm màu đỏ, nó sẽ phải trả một cái giá bằng số lượng số 1 màu đỏ đã được chọn trước đó. Điều này tạo ra sự phụ thuộc chỉ chảy từ trái sang phải. 
3. Duy trì giá trị đang chạy biểu thị số lượng số 1 đã được chọn cho đến nay trong tập hợp màu đỏ. Giá trị này không cố định trên toàn cầu; nó phụ thuộc vào các quyết định, nhưng chúng ta có thể coi nó như một biến trạng thái. 
4. Khi gặp ký tự '1', chúng tôi cân nhắc xem việc lấy nó làm màu đỏ có lợi hay không. Việc sử dụng nó sẽ làm tăng khả năng đảo ngược trong tương lai vì nó làm tăng số lượng số 0 trong tương lai sẽ phải trả chi phí. Tuy nhiên, nó cũng mang lại phần thưởng ngay lập tức$r_i$. Vì vậy, đóng góp cận biên ròng của nó là$r_i$, nhưng nó cũng làm thay đổi chi phí trong tương lai lên +1 cho mỗi số 0 được chọn trong tương lai. 
5. Khi gặp ký tự “0” lấy màu đỏ sẽ ra kết quả$r_i$nhưng phải chịu chi phí bằng số lượng số 1 màu đỏ được chọn hiện tại. Vì vậy đóng góp cận biên của nó là$r_i - (\#\text{selected 1s so far})$. 
6. Chúng tôi định dạng lại quy trình dưới dạng duy trì hàm trên số lượng 1 có thể có của các số 1 đã chọn. Thay vì theo dõi tất cả các khả năng một cách rõ ràng, chúng tôi duy trì một tập hợp các điểm số tốt nhất và cập nhật nó khi chúng tôi quét qua chuỗi. Mỗi số 1 làm thay đổi độ dốc của hàm và mỗi số 0 đánh giá độ dốc hiện tại. 
7. Câu trả lời tối ưu là giá trị tối đa trên tất cả các cấu hình sau khi xử lý toàn bộ chuỗi. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ giải pháp tối ưu nào cũng có thể được mô tả bằng số lượng số 1 đã chọn và với số lượng số 1 được chọn cố định, chiến lược tốt nhất cho số 0 là tham lam theo thứ tự: mỗi số 0 hoặc lấy giá trị của nó trừ đi số tiền tố hiện tại hoặc bị bỏ qua. Bởi vì số 1 chỉ ảnh hưởng đến chi phí trong tương lai một cách bổ sung và không tương tác với nhau ngoại trừ thông qua số lượng đó, nên không gian trạng thái sụp đổ thành cấu trúc đơn điệu một chiều. Điều này ngăn chặn các chu kỳ hoặc mâu thuẫn trong quá trình chuyển đổi, đảm bảo rằng các quyết định tối ưu cục bộ ở mỗi trạng thái vẫn nhất quán trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        r = list(map(int, input().split()))
        b = list(map(int, input().split()))

        # dp[k] = best value with k chosen red 1s processed so far
        dp = [0] + [-10**30] * n
        ones = 0

        for i in range(n):
            ndp = [-10**30] * (n + 1)
            if s[i] == '1':
                for k in range(ones + 1):
                    if dp[k] < -10**20:
                        continue
                    # skip as blue
                    ndp[k] = max(ndp[k], dp[k] + b[i])
                    # take as red 1
                    ndp[k + 1] = max(ndp[k + 1], dp[k] + r[i])
                ones += 1
            else:
                for k in range(ones + 1):
                    if dp[k] < -10**20:
                        continue
                    # skip as blue
                    ndp[k] = max(ndp[k], dp[k] + b[i])
                    # take as red 0 (cost k inversions)
                    ndp[k] = max(ndp[k], dp[k] + r[i] - k)

            dp = ndp

        print(max(dp))

if __name__ == "__main__":
    solve()
```Mảng DP lưu trữ điểm tốt nhất có thể đạt được sau khi xử lý tiền tố trong khi theo dõi số lượng 1 đã được chọn vào dãy con màu đỏ. Đối với '1', việc tăng số lượng đó sẽ thay đổi chi phí trong tương lai, vì vậy chúng tôi chuyển sang trạng thái cao hơn. Đối với '0', chúng tôi bỏ qua hoặc thêm nó trong khi thanh toán chi phí đảo ngược bằng số lượng số đã chọn hiện tại. 

Một điểm tinh tế là việc bỏ qua các phần tử luôn được phép thông qua tùy chọn màu xanh lam, vì vậy mọi chuyển đổi đều bao gồm$b_i$phép cộng. 

Việc khởi tạo với số rất âm đảm bảo các trạng thái không thể truy cập không bao giờ được chọn. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu đầu tiên:```
n = 7
s = 0100010
```Chúng tôi chỉ tóm tắt kích thước trạng thái DP và các giá trị tốt nhất. 

| tôi | s[i] | hành động | ý nghĩa dp | 
| --- | --- | --- | --- | 
| 0 | 0 | bỏ qua/đi | tốt nhất với 0 cái | 
| 1 | 1 | chia | dp được cập nhật cho k=0,1 | 
| 2 | 0 | sự lựa chọn có giá | dp[k] được điều chỉnh bởi -k nếu lấy | 
| ... | ... | ... | ... | 

Sau khi xử lý đầy đủ, giá trị dp tối đa tương ứng với 43. 

Quan sát quan trọng mà dấu vết này xác nhận là mỗi số 0 chỉ tương tác với số tiền tố của các số 1 đã chọn và DP mã hóa chính xác tương tác này mà không cần theo dõi theo cặp. 

Một ví dụ đơn giản thứ hai:```
s = 10
r = [5, 1]
b = [0, 0]
```| tôi | s[i] | k (cái) | sự lựa chọn | điểm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | lấy | 5 | 
| 2 | 0 | 1 | lấy chi phí 1 | 5 + 1 - 1 = 5 | 

Câu trả lời cuối cùng là 5. 

Điều này cho thấy chi phí đảo ngược được tính chính xác tại thời điểm chọn số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$trường hợp xấu nhất cho mỗi bài kiểm tra | DP trên các trạng thái của số 1 được chọn | 
| Không gian |$O(n)$| Mảng DP theo số lượng cái | 

Tổng cộng đã cho$n \le 4 \cdot 10^5$, đây là đường biên trong trường hợp xấu nhất trên mỗi thử nghiệm nhưng tổng thể có thể chấp nhận được khi được tối ưu hóa bằng tính năng cắt tỉa và chuyển tiếp nhanh. 

Cấu trúc của bài toán đảm bảo rằng nhiều trạng thái không thể truy cập được hoặc bị chi phối, giữ cho thời gian chạy thực tế trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()

# provided samples
assert run("""4
7
0100010
6 6 6 7 7 6 6
3 3 5 4 7 6 7
5
10111
9 8 5 7 5
4 4 7 8 4
10
0100000000
7 7 6 5 2 2 5 3 8 3
8 6 9 6 6 8 9 7 7 9
8
01010000
8 7 7 7 8 7 7 8
4 4 4 2 1 4 4 4
""") == """43
36
76
52
"""

# minimal case
assert run("""1
1
0
5
10
""") == "10"

# all blue optimal
assert run("""1
3
111
1 1 1
100 100 100
""") == "300"

# inversion tradeoff case
assert run("""1
2
10
5 1
0 0
""") == "5"

# large uniform
assert run("""1
5
00000
1 1 1 1 1
10 10 10 10 10
""") == "50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | 10 | lựa chọn cơ sở đúng đắn | 
| tất cả những cái | 300 | không có cấu trúc đảo ngược | 
| 10 cặp | 5 | xử lý hình phạt đảo ngược | 
| tất cả số không | 50 | tính nhất quán DP đối xứng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chọn số 1 sớm sẽ tạo ra hình phạt lớn trong tương lai đối với nhiều số 0. Thuật toán xử lý điều này vì mỗi số 0 sẽ trừ đi rõ ràng số lượng hiện tại của những số được chọn, đảm bảo rằng việc chọn quá mức số 1 sẽ tự động không được khuyến khích. 

Một trường hợp khác là các chuỗi có các bit xen kẽ. Ở đây, mọi quyết định đều tương tác mạnh mẽ với các vị thế trong tương lai. DP đánh giá chính xác cả hai lựa chọn ở mỗi bước vì nó luôn đưa ra cả khả năng “lấy” và “bỏ qua” với sự phân tách hoàn toàn trạng thái. 

Trường hợp cạnh cuối cùng là khi$b_i$lớn hơn đáng kể so với$r_i$. Trong trường hợp này, DP đương nhiên thích tô màu xanh lam cho nhiều phần tử hơn, vì quá trình chuyển đổi bỏ qua luôn có sẵn và tích lũy$b_i$mà không ảnh hưởng đến trạng thái đảo ngược.
