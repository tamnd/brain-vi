---
title: "CF 104764D - Bầy Thạch"
description: "Chúng ta có một tập hợp các vị trí số nguyên riêng biệt trên một dòng, mỗi vị trí đại diện cho một con sứa. Từ những vị trí này, chúng ta phải chọn chính xác $K$ trong số chúng và chỉ xem xét những điểm đã chọn."
date: "2026-06-28T21:41:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104764
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 11-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104764
solve_time_s: 62
verified: false
draft: false
---

[CF 104764D - Bầy Thạch](https://codeforces.com/problemset/problem/104764/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các vị trí số nguyên riêng biệt trên một dòng, mỗi vị trí đại diện cho một con sứa. Từ những vị trí này chúng ta phải chọn chính xác$K$của chúng và chỉ xem xét những điểm đã chọn. Trong số tất cả các lựa chọn có thể có của$K$điểm, chúng ta xem xét khoảng cách lớn nhất giữa hai điểm bất kỳ đã chọn và chúng ta muốn làm cho khoảng cách đó càng nhỏ càng tốt. 

Khi một tập hợp con được chọn, Jerry có thể đứng ở bất cứ đâu, nhưng điều đó không thay đổi thực tế là sự phân tán của nhóm chỉ được xác định bởi con sứa được chọn ngoài cùng bên trái và ngoài cùng bên phải. Khoảng cách tối đa bên trong bất kỳ tập hợp nào được chọn chỉ đơn giản là sự khác biệt giữa phần tử tối đa và tối thiểu của nó. Vì vậy, nhiệm vụ giảm xuống còn việc chọn một tập hợp con có kích thước$K$đó giảm thiểu phạm vi. 

Kích thước đầu vào đạt$2 \cdot 10^5$, do đó, bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc thậm chí tất cả các kết hợp đều không thể thực hiện được ngay lập tức. Quét bậc hai trên các cặp hoặc tập hợp con sẽ yêu cầu theo thứ tự$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. Chúng ta nên kỳ vọng nhiều nhất là một kết quả gần tuyến tính hoặc$N \log N$giải pháp. 

Một cạm bẫy ngây thơ là quên rằng chỉ những thái cực được chọn mới quan trọng. Ví dụ, nếu các vị trí là$[1, 2, 10, 11]$Và$K = 3$, đang chọn$\{1, 2, 11\}$cho phạm vi 10, trong khi$\{1, 2, 10\}$đưa ra phạm vi 9, mặc dù cả hai đều bao gồm các phần tử nhỏ giống nhau. Cấu trúc phụ thuộc hoàn toàn vào việc chọn ba điểm liên tiếp theo thứ tự sắp xếp. 

Một vấn đề tế nhị khác là giả định rằng vị trí của Jerry có tầm quan trọng đối với khoảng cách. Nó không. Bài toán chỉ yêu cầu khoảng cách tối đa giữa các con sứa, vì vậy Jerry không liên quan đến hàm mục tiêu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là liệt kê mọi tập hợp con có kích thước$K$, tính phần tử tối thiểu và tối đa của nó và lấy chênh lệch nhỏ nhất có thể. Điều này đúng vì định nghĩa của mục tiêu hoàn toàn mang tính tổ hợp trên các tập hợp con. Tuy nhiên, số lượng các tập con như vậy là$\binom{N}{K}$, trở nên rất lớn ngay cả đối với các giá trị vừa phải của$N$. Vì$N = 200000$, điều này hoàn toàn không khả thi, và ngay cả đối với$N = 40$nó đã quá lớn rồi. 

Quan sát cấu trúc quan trọng là khi các vị trí được sắp xếp, mọi tập hợp con tối ưu có kích thước$K$phải bao gồm$K$phần tử liên tiếp theo thứ tự sắp xếp đó. Nếu một tập hợp con bỏ qua một phần tử trong phạm vi của nó, việc thay thế phần tử được chọn lớn hơn bằng phần tử nhỏ hơn bị bỏ qua chỉ có thể giảm phạm vi hoặc giữ nguyên phần tử đó. Tính đơn điệu này thu gọn không gian tìm kiếm từ tổ hợp sang tuyến tính trên các cửa sổ. 

Do đó, sau khi sắp xếp, bài toán sẽ quét tất cả các cửa sổ liền kề có độ dài$K$và tính toán sự khác biệt giữa các điểm cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{N}{K} \cdot K)$|$O(K)$| Quá chậm | 
| Cửa sổ trượt trên mảng được sắp xếp |$O(N \log N)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các vị trí sứa theo thứ tự tăng dần. Điều này đảm bảo mức độ lan rộng của bất kỳ nhóm nào được xác định bởi cấu trúc liền kề theo thứ tự này thay vì lựa chọn tùy ý. 
2. Khởi tạo biến trả lời có giá trị rất lớn. Điều này sẽ theo dõi phạm vi tốt nhất (nhỏ nhất) gặp phải. 
3. Lặp lại mọi chỉ mục$i$sao cho một cửa sổ có kích thước$K$bắt đầu từ$i$phù hợp bên trong mảng. Đối với mỗi$i$, hãy xem xét nhóm được hình thành bởi các phần tử từ$i$ĐẾN$i + K - 1$. 
4. Tính phạm vi của nhóm này là$a[i + K - 1] - a[i]$. Điều này hợp lệ vì trong một mảng được sắp xếp, giá trị tối đa và tối thiểu của một đoạn liền kề là điểm cuối của nó. 
5. Cập nhật câu trả lời với giá trị tối thiểu trên tất cả các cửa sổ như vậy. 

Sau khi quét xong, câu trả lời được lưu trữ là phạm vi nhỏ nhất có thể. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, giả sử một tập hợp con tối ưu không liền kề nhau theo thứ tự chỉ mục. Khi đó tồn tại ít nhất một phần tử bên trong khoảng giữa mức tối thiểu và tối đa của nó không được chọn. Việc thay thế một trong các phần tử cực trị đã chọn bằng phần tử bên trong bị thiếu này không thể tăng phạm vi vì nó di chuyển điểm cuối vào trong hoặc giữ cho điểm cuối không thay đổi. Việc lặp lại đối số này sẽ biến đổi bất kỳ tập hợp con tối ưu nào thành một khối liền kề mà không làm giảm giá trị mục tiêu của nó. Do đó việc hạn chế sự chú ý vào các phân đoạn liên tiếp không làm mất đi tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    ans = float('inf')
    
    for i in range(n - k + 1):
        ans = min(ans, a[i + k - 1] - a[i])
    
    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc đầu vào và sắp xếp danh sách vị trí. Việc sắp xếp là cần thiết vì nó chuyển đổi bài toán hình học trên một đường thành bài toán mảng có cấu trúc trong đó các cửa sổ đại diện cho các nhóm ứng cử viên. 

Vòng lặp trên các chỉ số bắt đầu liệt kê tất cả các kích thước hợp lệ-$K$phân đoạn. Đối với mỗi phân khúc, chúng tôi tính toán mức chênh lệch của nó trực tiếp từ các điểm cuối. Mức tối thiểu trên tất cả các chênh lệch như vậy được lưu trữ. Việc sử dụng một lần duy nhất đảm bảo quét tuyến tính sau khi sắp xếp. 

Một lỗi phổ biến là cố gắng duy trì một cửa sổ động mà không sắp xếp trước. Điều đó phá vỡ thuộc tính điểm cuối và dẫn đến tính toán phạm vi không chính xác. Một sai lầm khác là tính toán sai các chỉ số, đặc biệt là quên rằng điểm bắt đầu hợp lệ cuối cùng là$n - k$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
8 6 1 5 5
```Mảng được sắp xếp trở thành$[1, 5, 5, 6, 8]$. 

Chúng tôi đánh giá các cửa sổ: 

| tôi | Cửa sổ | Phạm vi | 
| --- | --- | --- | 
| 0 | [1, 5, 5] | 4 | 
| 1 | [5, 5, 6] | 1 | 
| 2 | [5, 6, 8] | 3 | 

Phạm vi tối thiểu là 1. 

Điều này cho thấy rằng việc phân cụm xung quanh các giá trị lặp lại hoặc đóng làm giảm đáng kể mức độ lây lan và nhóm tối ưu luôn xuất hiện từ một phân đoạn liền kề. 

### Mẫu 2 

đầu vào:```
7 4
1 2 4 5 6 7 9
```Mảng được sắp xếp giống hệt nhau. 

| tôi | Cửa sổ | Phạm vi | 
| --- | --- | --- | 
| 0 | [1, 2, 4, 5] | 4 | 
| 1 | [2, 4, 5, 6] | 4 | 
| 2 | [4, 5, 6, 7] | 3 | 
| 3 | [5, 6, 7, 9] | 4 | 

Câu trả lời là 3. 

Điều này xác nhận rằng cửa sổ tối ưu không nhất thiết phải ở một đầu của mảng mà ở bất kỳ đâu có mật độ cục bộ cao nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian |$O(1)$thêm | chỉ sắp xếp và một vài biến được sử dụng | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử và$O(N \log N)$việc sắp xếp nằm trong giới hạn trong Python. Việc quét tuyến tính sau đó là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    ans = float('inf')
    for i in range(n - k + 1):
        ans = min(ans, a[i + k - 1] - a[i])
    return str(ans)

# provided samples
assert run("5 3\n8 6 1 5 5\n") == "1"
assert run("7 4\n1 2 4 5 6 7 9\n") == "3"

# custom cases
assert run("1 1\n100\n") == "0", "single element"
assert run("4 2\n1 10 20 30\n") == "9", "smallest pair dominates"
assert run("5 5\n1 2 3 4 5\n") == "4", "take all elements"
assert run("6 3\n1 2 2 100 101 102\n") == "0", "tight cluster"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cạnh tối thiểu | 
| số cách nhau | 9 | cửa sổ cặp đúng | 
| lấy tất cả | 4 | xử lý toàn dải | 
| giá trị nhóm | 0 | phát hiện cửa sổ cục bộ tốt nhất | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$K = 1$. Phạm vi của bất kỳ phần tử đơn lẻ nào đều bằng 0 và thuật toán xử lý điều này vì mọi cửa sổ có kích thước 1 đều tạo ra$a[i] - a[i] = 0$. 

Một trường hợp khác là khi tất cả các giá trị giống hệt nhau, mặc dù câu lệnh đảm bảo các vị trí khác nhau. Nếu hạn chế đó được loại bỏ, cửa sổ trượt vẫn trả về 0 một cách chính xác cho bất kỳ$K$, vì tất cả các điểm cuối đều khớp. 

Khoảng cách lớn giữa các cụm kiểm tra xem thuật toán có tránh chọn các điểm cực đoan một cách chính xác hay không. Ví dụ,$[1, 2, 3, 100, 101]$với$K = 3$mang lại cửa sổ tối ưu$[1,2,3]$hoặc$[100,101, \text{(invalid)}]$tùy thuộc vào vị trí và quá trình quét sẽ chọn chính xác vùng dày đặc thay vì các điểm cuối trải rộng trên toàn bộ phạm vi. 

Thuật toán xử lý tất cả những điều này một cách tự nhiên vì mọi ứng viên đều được đánh giá thống nhất thông qua sự khác biệt về điểm cuối sau khi sắp xếp.
