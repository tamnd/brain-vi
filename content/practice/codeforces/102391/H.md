---
title: "CF 102391H - Bộ tối đa hóa"
description: "Chúng ta bắt đầu với hai hoán vị (A) và (B), cả hai đều chứa mọi số nguyên từ (1) đến (N) đúng một lần. Chúng ta có thể sắp xếp lại (A), nhưng chỉ bằng cách hoán đổi các phần tử lân cận. Mục tiêu không phải là xây dựng một sự sắp xếp tối đa cụ thể."
date: "2026-08-10T20:59:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "H"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 220
verified: true
draft: false
---

[CF 102391H - Trình tối đa hóa](https://codeforces.com/problemset/problem/102391/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai hoán vị (A) và (B), cả hai đều chứa mọi số nguyên từ (1) đến (N) đúng một lần. Chúng ta có thể sắp xếp lại (A), nhưng chỉ bằng cách hoán đổi các phần tử lân cận. Mục tiêu không phải là xây dựng một sự sắp xếp tối đa cụ thể. Chúng ta cần số lượng hoán đổi liền kề nhỏ nhất để biến đổi (A) ban đầu thành bất kỳ sự sắp xếp nào có giá trị 

[ 
\sum_{i=1}^{N}|a_i-b_i| 
] 

là càng lớn càng tốt. 

Bởi vì các hoán vị liền kề có thể tạo ra mọi hoán vị, nên vấn đề thực sự là mô tả đặc điểm của tất cả các hoán vị làm tối đa hóa tổng, sau đó tìm hoán vị gần nhất với hoán vị ban đầu (A) trong khoảng cách hoán đổi liền kề. 

Giới hạn (N\le 250000) loại trừ bất kỳ số hạng bậc hai nào trong (N), chưa nói đến việc liệt kê các hoán vị. Ngay cả (O(N^2)) cũng sẽ yêu cầu khoảng (6,25\times10^{10}) thao tác cơ bản ở kích thước tối đa, vượt xa giới hạn hai giây. Về cơ bản chúng ta cần thời gian tuyến tính hoặc nhiều nhất là (O(N\log N)). May mắn thay, (A) và (B) là các hoán vị của chính xác (1,\ldots,N), vì vậy giá trị của chúng đã cho chúng ta thứ hạng của chúng. Không có sự sắp xếp nào thực sự cần thiết. 

Có một số trường hợp nhỏ mà giải pháp hấp dẫn không thành công. Đối với (N=3), (A=[1,2,3]), (B=[1,2,3]), hoán vị ngược ([3,2,1]) là tối ưu, nhưng nó cần ba lần hoán đổi liền kề. Câu trả lời đúng chỉ là (2), vì ([2,3,1]) và ([3,1,2]) cũng tối ưu. Một giải pháp giả định hoán vị ngược là tối ưu duy nhất sẽ mắc sai lầm này. 

Với (N=1), với (A=[1]) và (B=[1]), câu trả lời là (0). Không có gì để trao đổi, và sự sắp xếp khả thi duy nhất đã là tối ưu. Bất kỳ cách triển khai nào coi hai nửa giá trị là không trống một cách mù quáng đều phải xử lý trường hợp này một cách riêng biệt. 

Với (N=2), (A=[1,2]), (B=[1,2]), câu trả lời là (1). Sự sắp xếp tối đa hóa duy nhất là ([2,1]). Điều này phát hiện các triển khai sử dụng quy tắc giá trị trung bình không chính xác cho chẵn (N). 

Cuối cùng, câu trả lời có thể lớn hơn nhiều so với (N). Đối với (N=250000), (A=B=[1,2,\ldots,N]), số lần hoán đổi được yêu cầu là (125000^2=15625000000). Số nguyên 32 bit sẽ tràn trong trường hợp này. Số nguyên Python có độ chính xác tùy ý, do đó việc triển khai không cần loại số nguyên đặc biệt. 

Yêu cầu về trường hợp kiểm tra "tất cả các giá trị bằng nhau" không thể được thỏa mãn theo nghĩa đen trong khi vẫn giữ đầu vào hợp lệ, bởi vì (A) và (B) là các hoán vị và do đó không chứa các giá trị lặp lại. Trường hợp liên quan gần nhất là (N=1), trong đó toàn bộ hoán vị bao gồm một giá trị (1). 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực có thể liệt kê mọi hoán vị (C) của (1,\ldots,N), tính toán 

[ 
\sum_i |c_i-b_i|, 
] 

giữ giá trị tối đa và trong số tất cả các công cụ tối đa hóa, hãy chọn giá trị yêu cầu ít giao dịch hoán đổi liền kề nhất từ ​​(A). Điều này đúng vì các hoán đổi liền kề có thể đạt tới mọi hoán vị và số lượng hoán đổi liền kề giữa hai hoán vị có thể được tính từ thứ tự tương đối của chúng. Vấn đề là số lượng ứng viên. Có (N!) hoán vị và việc đánh giá mỗi hoán vị thực hiện (N) thao tác, mang lại kết quả (O(N\cdot N!)). Tại (N=10), đây đã là khoảng (36) triệu so sánh giá trị và sự tăng trưởng giai thừa khiến cách tiếp cận này trở nên vô dụng đối với giới hạn thực tế. 

Quan sát hữu ích đầu tiên là viết lại mục tiêu. Kể từ khi 

[ 
|x-y|=x+y-2\min(x,y), 
] 

chúng tôi có 

\sum_i c_i+\sum_i b_i-2\sum_i\min(c_i,b_i). 
] 

Cả hai hoán vị đều chứa (1,\ldots,N), nên hai tổng đầu tiên là cố định. Việc tối đa hóa biểu thức ban đầu hoàn toàn giống với việc tối thiểu hóa 

[ 
\sum_i\min(c_i,b_i). 
] 

Bây giờ hãy mở rộng từng mức tối thiểu theo ngưỡng: 

[ 
\min(x,y)=\sum_{t=1}^{N}[x\ge t\text{ và }y\ge t]. 
] 

Đối với một ngưỡng cố định (t), chính xác (N-t+1) vị trí chứa ít nhất một giá trị (t) trong (C) và chính xác cùng số vị trí có (b_i\ge t). Nếu hai tập con của một tập hợp phần tử (N) đều chứa các phần tử (k), phần giao của chúng có kích thước ít nhất là 

[ 
\max(0,2k-N). 
]

Do đó, mọi ngưỡng đều có giới hạn dưới đối với mức đóng góp của nó vào (\sum_i\min(c_i,b_i)). 

Ngưỡng quan trọng là ở khoảng giữa. Đặt (N=2m). Các giá trị (m) lớn nhất của (C), cụ thể là (m+1,\ldots,2m), phải chiếm các vị trí có giá trị (B) nằm trong số các giá trị (m) nhỏ nhất. Nếu không, hai tập hợp vị trí (m) sẽ chồng lên nhau, làm cho ngưỡng đóng góp lớn hơn mức tối thiểu của nó. 

Lý do tương tự cho số lẻ (N=2m+1) nói rằng các giá trị (m+2,\ldots,2m+1) phải chiếm các vị trí trong đó (B) nằm trong số (1,\ldots,m), các giá trị (1,\ldots,m) phải chiếm các vị trí trong đó (B) nằm trong số (m+2,\ldots,2m+1) và giá trị ở giữa (m+1) phải được ghép với giá trị ở giữa (B=m+1). 

Trong mỗi nhóm, thứ tự chính xác không ảnh hưởng đến mức tối đa. Đó là sự đơn giản hóa quan trọng. Chúng ta không cần phải quyết định giá trị lớn cụ thể nào sẽ rơi vào vị trí (B) thấp nào. Chúng ta chỉ cần di chuyển các phần tử vào đúng nhóm. 

Điều này chuyển đổi vấn đề hoán vị ban đầu thành một vấn đề đơn giản hơn nhiều trên các chuỗi danh mục. Với (N) chẵn, mọi giá trị đều thuộc về nửa thấp hoặc nửa cao. Đối với số lẻ (N), có thêm một danh mục ở giữa chứa chính xác một giá trị. Danh mục mục tiêu của mọi vị trí được xác định trực tiếp bởi giá trị (B) của nó. 

Có thể đạt được số lượng hoán đổi liền kề tối thiểu cần thiết để chuyển đổi một chuỗi danh mục thành một chuỗi khác bằng cách khớp lần xuất hiện đầu tiên của mỗi danh mục với lần xuất hiện mục tiêu đầu tiên, lần xuất hiện thứ hai với lần xuất hiện mục tiêu thứ hai, v.v. Đối với một danh mục cố định, việc vượt qua hai lần xuất hiện của cùng một danh mục đó không thể giúp ích được gì, vì vậy việc kết hợp chúng theo thứ tự là tối ưu. Nếu vị trí xuất hiện hiện tại là (p_1,p_2,\ldots) và vị trí mục tiêu là (q_1,q_2,\ldots), thì sự đóng góp của chúng vào tổng chuyển động vị trí là 

[ 
\sum_j |p_j-q_j|. 
] 

Mỗi lần hoán đổi liền kề sẽ di chuyển hai phần tử theo một vị trí, do đó tổng của tất cả các danh mục sẽ tính mỗi lần hoán đổi hai lần. Do đó, câu trả lời là một nửa của toàn bộ chuyển động vị trí. 

Do đó, thuật toán cuối cùng là tuyến tính. Chúng tôi phân loại mọi giá trị trong (A), phân loại mọi vị trí theo giá trị (B) của nó, thu thập các vị trí xuất hiện cho từng danh mục và khớp các lần xuất hiện tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N\cdot N!)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt (m=\lfloor N/2\rfloor). Xác định loại cho các giá trị của (A). Các giá trị (1,\ldots,m) thuộc loại thấp, các giá trị (m+2,\ldots,N) thuộc loại cao và khi (N) là số lẻ, giá trị (m+1) là loại giữa. 

Sự phân loại này nắm bắt chính xác ba nhóm có thể xuất hiện trong một hoán vị tối ưu. 
2. Phân loại từng vị trí theo giá trị của nó trong (B). Nếu (B_i\le m), sự sắp xếp tối ưu phải đặt giá trị (A) cao ở đó. Nếu (B_i>m+1), nó phải đặt giá trị (A) thấp ở đó. Đối với số lẻ (N), vị trí duy nhất có (B_i=m+1) phải nhận giá trị ở giữa. 

Đối với số chẵn (N), không có loại ở giữa nên điều kiện chỉ đơn giản là hoán đổi nửa thấp và nửa cao. 
3. Quét (A) và ghi lại các vị trí mà mỗi danh mục hiện đang xuất hiện. Đồng thời quét các danh mục mục tiêu do (B) tạo ra và ghi lại các vị trí mà mỗi danh mục phải xuất hiện. 

Các giá trị chính xác bên trong một danh mục không quan trọng. Đây là lý do tại sao chỉ ghi lại các danh mục là đủ. 
4. Đối với mỗi danh mục, hãy ghép các lần xuất hiện hiện tại và các lần xuất hiện mục tiêu theo thứ tự từ trái sang phải. Thêm sự khác biệt tuyệt đối giữa mọi vị trí được ghép nối. 

Các lần xuất hiện trùng khớp theo thứ tự khác sẽ buộc hai phần tử cùng loại phải giao nhau. Vì chúng có thể thay thế cho nhau về mặt mục tiêu nên việc vượt qua như vậy chỉ có thể thêm các giao dịch hoán đổi và không thể cải thiện kết quả. 
5. Chia tổng số chuyển động vị trí cho hai và in nó. 

Mỗi lần hoán đổi liền kề sẽ di chuyển một phần tử sang trái một vị trí và một vị trí khác sang phải. Do đó, nó đóng góp chính xác (2) vào tổng số thay đổi vị trí tuyệt đối. 

### Tại sao nó hoạt động

Đối số ngưỡng chứng minh rằng mọi hoán vị tối đa hóa đều có chính xác danh mục được yêu cầu ở mọi vị trí. Ngược lại, bất kỳ hoán vị nào đáp ứng các yêu cầu danh mục đó đều đạt đến tất cả các giới hạn dưới của ngưỡng cùng một lúc, do đó, nó đang tối đa hóa. Sau khi các danh mục được cố định, các giá trị thuộc cùng một danh mục có thể thay thế cho mục tiêu. Chuyển đổi hoán đổi liền kề tối thiểu giữa hai chuỗi danh mục có được bằng cách khớp các lần xuất hiện danh mục bằng nhau theo thứ tự. Do đó, chuyển động vị trí được tính toán chính xác gấp đôi số lần hoán đổi tối thiểu và thuật toán trả về mức tối thiểu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    m = n // 2

    # Categories:
    # 0 = low values
    # 1 = middle value, only for odd n
    # 2 = high values
    #
    # Store positions of each category in the current A
    # and in the required target category sequence.
    cur = [[], [], []]
    target = [[], [], []]

    for i, x in enumerate(a):
        if x <= m:
            cur[0].append(i)
        elif n % 2 == 1 and x == m + 1:
            cur[1].append(i)
        else:
            cur[2].append(i)

    for i, x in enumerate(b):
        if x <= m:
            # Small B values need large A values.
            target[2].append(i)
        elif n % 2 == 1 and x == m + 1:
            # The middle value must meet the middle value.
            target[1].append(i)
        else:
            # Large B values need small A values.
            target[0].append(i)

    movement = 0

    for c in range(3):
        for p, q in zip(cur[c], target[c]):
            movement += abs(p - q)

    print(movement // 2)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên phân loại các phần tử của (A) theo giá trị số của chúng. Đối với (N) chẵn, nửa dưới là (1,\ldots,N/2), trong khi mọi giá trị lớn hơn đều thuộc về nửa cao. Đối với số lẻ (N), giá trị đơn ((N+1)/2) nhận được danh mục riêng. 

Vòng lặp thứ hai thực hiện phân loại tương tự từ hướng khác. (B_i) nhỏ yêu cầu giá trị (A) lớn trong sự sắp xếp tối ưu, trong khi (B_i) lớn yêu cầu giá trị (A) nhỏ. Đối với lẻ (N), vị trí ở giữa phải nhận giá trị ở giữa. 

Các vị trí được lưu trữ dưới dạng chỉ số dựa trên số không. Điều này thuận tiện vì khoảng cách giữa hai vị trí vẫn chính xác bằng số lần hoán đổi liền kề cần thiết để di chuyển một phần tử giữa chúng. Không cần điều chỉnh ranh giới. 

các`zip`các cặp thao tác xuất hiện theo thứ tự tự nhiên từ trái sang phải. Cả hai danh sách đều chứa cùng số lần xuất hiện vì cả (A) và (B) đều là hoán vị. Tổng chuyển động được tích lũy trên tất cả các danh mục và chia cho hai vì mỗi lần hoán đổi liền kề sẽ thay đổi vị trí của chính xác hai phần tử. 

Câu trả lời có thể vượt quá (2^{31}-1), nhưng số nguyên Python sẽ tự động mở rộng khi cần thiết. Câu trả lời lớn nhất có thể là theo thứ tự (N^2), có thể dễ dàng xử lý bằng số học số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3
1 2 3
1 2 3
```Ở đây (N=3) và (m=1). Giá trị (1) thấp, (2) trung bình và (3) cao. 

| Vị trí | Một giá trị | Danh mục hiện tại | Giá trị B | Danh mục mục tiêu | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | Thấp | 1 | Cao | 
| 1 | 2 | Trung | 2 | Trung | 
| 2 | 3 | Cao | 3 | Thấp | 

Loại thấp hiện đang ở vị trí (0) nhưng phải ở vị trí (2), đóng góp (2). Loại cao chuyển từ vị trí (2) sang vị trí (0), đóng góp vị trí khác (2). Hạng trung không di chuyển. 

| Danh mục | Vị trí hiện tại | Vị trí mục tiêu | Phong trào | 
| --- | --- | --- | --- | 
| Thấp | [0] | [2] | 2 | 
| Trung | [1] | [1] | 0 | 
| Cao | [2] | [0] | 2 | 

Tổng chuyển động là (4), vì vậy câu trả lời là (4/2=2). 

Điều này chứng tỏ tại sao việc đảo chiều (A) là không cần thiết. Mục tiêu có thể là ([2,3,1]), có cùng mục tiêu tối đa và chỉ yêu cầu hai lần hoán đổi. 

### Mẫu 2 

Đầu vào là```
4
3 4 1 2
3 2 4 1
```Bây giờ (N=4) và (m=2). Giá trị (1,2) thấp, trong khi (3,4) cao. 

| Vị trí | Một giá trị | Danh mục hiện tại | Giá trị B | Danh mục mục tiêu | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | Cao | 3 | Thấp | 
| 1 | 4 | Cao | 2 | Cao | 
| 2 | 1 | Thấp | 4 | Thấp | 
| 3 | 2 | Thấp | 1 | Cao | 

Các giá trị cao hiện chiếm vị trí (0,1), nhưng chúng cần vị trí (1,3). Chuyển động của họ là 

[ 
|0-1|+|1-3|=3. 
] 

Các giá trị thấp di chuyển từ vị trí (2,3) đến vị trí (0,2), cũng đóng góp (3). 

| Danh mục | Vị trí hiện tại | Vị trí mục tiêu | Phong trào | 
| --- | --- | --- | --- | 
| Thấp | [2, 3] | [0, 2] | 3 | 
| Cao | [0, 1] | [1, 3] | 3 | 

Tổng chuyển động là (6), do đó số lần hoán đổi tối thiểu là (6/2=3). 

Điều này cũng cho thấy tại sao chúng ta không cần phải quyết định xem giá trị cao cuối cùng sẽ là (3,4) hay (4,3) ở vị trí mục tiêu của chúng. Cả hai lựa chọn đều tốt như nhau và việc so khớp lần xuất hiện sẽ tự động chọn thứ tự rẻ hơn so với thứ tự ban đầu (A). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi phần tử của (A) và (B) được phân loại một lần và mọi vị trí được lưu trữ đều được xử lý một lần. | 
| Không gian | (O(N)) | Vị trí xuất hiện hiện tại và mục tiêu cùng chứa các vị trí (O(N)). | 

Độ phức tạp tuyến tính nằm trong giới hạn (N\le250000). Thuật toán chỉ thực hiện một vài lần chuyển qua đầu vào và không sắp xếp hoặc sử dụng cây Fenwick, cây phân đoạn hoặc cấu trúc đồ thị. Mức tiêu thụ bộ nhớ cũng tuyến tính và thấp hơn giới hạn 1024 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    m = n // 2

    cur = [[], [], []]
    target = [[], [], []]

    for i, x in enumerate(a):
        if x <= m:
            cur[0].append(i)
        elif n % 2 == 1 and x == m + 1:
            cur[1].append(i)
        else:
            cur[2].append(i)

    for i, x in enumerate(b):
        if x <= m:
            target[2].append(i)
        elif n % 2 == 1 and x == m + 1:
            target[1].append(i)
        else:
            target[0].append(i)

    movement = 0
    for c in range(3):
        for p, q in zip(cur[c], target[c]):
            movement += abs(p - q)

    print(movement // 2)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        input = sys.stdin.readline

        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = sys.stdin.readline

# Provided samples
assert run("""3
1 2 3
1 2 3
""") == "2", "sample 1"

assert run("""4
3 4 1 2
3 2 4 1
""") == "3", "sample 2"

# Minimum size
assert run("""1
1
1
""") == "0", "single element"

# Even N, exactly one required swap
assert run("""2
1 2
1 2
""") == "1", "even boundary"

# Odd N, middle value must stay with the middle B-value
assert run("""5
1 2 3 4 5
1 2 3 4 5
""") == "6", "odd middle case"

# Already has an optimal category arrangement
assert run("""4
3 4 1 2
1 2 3 4
""") == "0", "already optimal"

# Maximum-size case and a large answer.
# For A=B=1..N with N=250000, the first half must cross
# with the second half, requiring (N/2)^2 swaps.
n = 250000
a = " ".join(map(str, range(1, n + 1)))
large_input = f"{n}\n{a}\n{a}\n"
assert run(large_input) == "15625000000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (N=1,\ A=[1],\ B=[1]) | 0 | Kích thước tối thiểu và trường hợp một giá trị | 
| (N=2,\ A=[1,2],\ B=[1,2]) | 1 | Ranh giới kích thước chẵn và hoán đổi một nửa hoàn chỉnh | 
| (N=5,\ A=[1,2,3,4,5],\ B=[1,2,3,4,5]) | 6 | Kích thước lẻ và giá trị trung bình cố định | 
| (N=4,\ A=[3,4,1,2],\ B=[1,2,3,4]) | 0 | Một sự sắp xếp vốn đã tối ưu | 
| (N=250000,\ A=B=[1,\ldots,N]) | 15625000000 | Kích thước đầu vào tối đa và câu trả lời số nguyên lớn | 

Kịch bản có giá trị bằng nhau được yêu cầu cố tình vắng mặt trong các thử nghiệm thực thi vì nó vi phạm điều kiện hoán vị. Một hoán vị hợp lệ không thể chứa các giá trị bằng nhau. Thay vào đó, thử nghiệm (N=1) bao gồm trường hợp nhỏ nhất có thể. 

## Vỏ cạnh 

Với (N=1), đầu vào```
1
1
1
```chỉ có một cách sắp xếp duy nhất. Danh mục ở giữa chứa một giá trị duy nhất, vị trí hiện tại và vị trí mục tiêu của nó đều bằng 0 và chuyển động bằng 0. Thuật toán in (0). 

Đối với ranh giới chẵn (N=2),```
2
1 2
1 2
```vị trí (B) đầu tiên có giá trị thấp và do đó yêu cầu giá trị cao (2). Vị trí thứ hai yêu cầu giá trị thấp (1). Trình tự danh mục hiện tại là Thấp, Cao, trong khi mục tiêu là Cao, Thấp. Hai lần xuất hiện trong danh mục di chuyển theo từng vị trí, đưa ra tổng chuyển động (2) và câu trả lời (1). 

Đối với trường hợp lẻ (N=3),```
3
1 2 3
1 2 3
```giá trị ở giữa (2) phải giữ nguyên ở vị trí giữa, trong khi (1) và (3) trao đổi danh mục. Lần xuất hiện thấp và cao mỗi lần di chuyển hai vị trí, tạo ra chuyển động tổng thể (4) và câu trả lời (2). Đây chính xác là trường hợp việc chọn mù quáng hoán vị ngược hoàn toàn sẽ đánh giá quá cao số lần hoán đổi cần thiết. 

Đối với cấu hình đã tối ưu,```
4
3 4 1 2
1 2 3 4
```hai vị trí đầu tiên có giá trị (B) nhỏ và đã chứa giá trị (A) cao (3,4). Hai vị trí cuối cùng có giá trị (B) lớn và đã chứa các giá trị thấp (1,2). Mọi vị trí danh mục hiện tại đều khớp với vị trí mục tiêu của nó, do đó chuyển động bằng 0 và không cần hoán đổi. 

Đối với trường hợp kích thước tối đa, lấy (N=250000) với (A=B=[1,2,\ldots,N]). Vị trí đầu tiên (125000) hiện chứa giá trị thấp nhưng phải chứa giá trị cao, trong khi vị trí cuối cùng (125000) chứa giá trị cao nhưng phải chứa giá trị thấp. Mỗi lần xuất hiện thấp phải vượt qua mọi lần xuất hiện cao, tạo ra 

[ 
125000\cdot125000=15625000000 
] 

hoán đổi liền kề. Thuật toán thu được chính xác giá trị này mà không cần xây dựng hoán vị cuối cùng.
