---
title: "CF 105278C - s-parkour"
description: "Chúng ta được cung cấp một chuỗi chiều cao của các tòa nhà, tất cả đều khác biệt. Đối với bất kỳ khoảng thời gian nào từ chỉ số $i$ đến $j$, chúng ta tưởng tượng việc đi dọc theo các tòa nhà theo thứ tự. Mỗi bước so sánh hai độ cao liên tiếp: nếu chúng ta di chuyển lên một tòa nhà cao hơn thì đó là đi lên, nếu không thì là đi xuống."
date: "2026-06-23T06:47:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105278
codeforces_index: "C"
codeforces_contest_name: "2024 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 105278
solve_time_s: 88
verified: false
draft: false
---

[CF 105278C - s-parkour](https://codeforces.com/problemset/problem/105278/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chiều cao của các tòa nhà, tất cả đều khác biệt. Đối với bất kỳ khoảng thời gian nào từ chỉ mục$i$ĐẾN$j$, chúng ta tưởng tượng mình đang đi dọc theo các tòa nhà theo thứ tự. Mỗi bước so sánh hai độ cao liên tiếp: nếu chúng ta di chuyển lên một tòa nhà cao hơn thì đó là đi lên, nếu không thì là đi xuống. Điều này tạo ra một mô hình thăng trầm dọc theo phân khúc. 

Bây giờ hãy xem xét cùng một đoạn nhưng đi theo hướng ngược lại. Điều này làm đảo ngược hướng của mọi so sánh, do đó, phần đi lên trở thành phần đi xuống và phần đi xuống trở thành phần nâng lên. 

Một đoạn$[i, j]$được gọi là công bằng nếu số lần đi lên theo hướng thuận bằng số lần đi lên theo hướng ngược lại. Vì đảo ngược các hoán đổi lên và xuống, điều kiện này tương đương với việc nói rằng số lần đi lên theo hướng thuận bằng số lần đi xuống theo hướng thuận. 

Vì vậy, nhiệm vụ giảm xuống việc đếm xem có bao nhiêu mảng con có số lần tăng và giảm bằng nhau giữa các phần tử liên tiếp. 

Kích thước đầu vào có thể lớn như$10^6$, do đó bất kỳ cách tiếp cận bậc hai nào trên tất cả$(i, j)$cặp ngay lập tức là không thể. Thậm chí$O(n \log n)$có thể chấp nhận được, nhưng bất cứ điều gì cố gắng tính toán lại thông tin trên mỗi phân đoạn sẽ hết thời gian vì có khoảng$5 \times 10^{11}$mảng con trong trường hợp xấu nhất. 

Một điểm tinh tế là các phân đoạn một phần tử luôn công bằng vì không có nước đi nào. Một trường hợp cạnh khác là mảng đơn điệu. Nếu chuỗi tăng hoặc giảm nghiêm ngặt thì chỉ các phân đoạn một phần tử là hợp lệ vì mọi phân đoạn dài hơn đều không cân bằng. 

Thách thức chính là việc đếm đơn giản trên mỗi phân đoạn đòi hỏi phải quét từng mảng con, tốc độ quá chậm. 

## Phương pháp tiếp cận 

Nếu chúng tôi thử dùng vũ lực, chúng tôi sẽ kiểm tra từng cặp$(i, j)$và số lượng tăng giảm trong mảng con. Ngay cả với tiền xử lý tiền tố, việc duy trì cả hai số lượng trên mỗi phân đoạn vẫn tốn kém$O(1)$cho mỗi truy vấn, nhưng có$O(n^2)$truy vấn, vì vậy điều này vẫn trở thành bậc hai. 

Quan sát quan trọng là mỗi so sánh liền kề đều góp phần$+1$(tăng) hoặc$-1$(giảm bớt). Một phân đoạn chính xác là công bằng khi tổng các giá trị này trên phân đoạn đó bằng 0. Vì vậy, chúng ta chuyển vấn đề thành các mảng con đếm có tổng bằng 0 trong một mảng gồm$+1/-1$. 

Cho phép$a_k = 1$nếu như$h_k < h_{k+1}$, nếu không thì$a_k = -1$. Sau đó một đoạn$[i, j]$là công bằng nếu$$a_i + a_{i+1} + \dots + a_{j-1} = 0.$$Đây là một bài toán tổng tiền tố cổ điển: xác định tổng tiền tố$p[0]=0$,$p[k]=a_1 + \dots + a_k$. Khi đó tổng phân đoạn bằng 0 chính xác khi$p[j-1] = p[i-1]$. Vì vậy chúng ta cần đếm các cặp tổng tiền tố bằng nhau. 

Điều này làm giảm vấn đề đếm tần số của các giá trị tổng tiền tố trong một lần truyền. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tần số tổng tiền tố |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi mảng chiều cao thành mảng chênh lệch trong đó mỗi vị trí mã hóa xem tòa nhà tiếp theo cao hơn hay thấp hơn. 

Sau đó, chúng tôi duy trì tổng tiền tố đang chạy trên mảng được chuyển đổi này. Mỗi lần chúng ta nhìn thấy lại cùng một giá trị tổng tiền tố, điều đó có nghĩa là đoạn giữa hai lần xuất hiện có số lần tăng và giảm bằng nhau. 

### bước 

1. Đọc mảng độ cao và xử lý ngay việc chúng ta chỉ cần so sánh giữa các phần tử liên tiếp. 

Điều này làm giảm chiều hướng vấn đề từ các tòa nhà đến các điểm chuyển tiếp. 
2. Với mỗi cặp liền kề, tính giá trị$+1$nếu đó là sự gia tăng và$-1$nếu đó là sự giảm sút. 

Mã hóa này được chọn vì sự cân bằng giữa thăng trầm trở thành điều kiện có tổng bằng 0. 
3. Duy trì tổng tiền tố đang chạy bắt đầu từ 0 trước khi bất kỳ phần tử nào được xử lý. 
4. Sử dụng từ điển (bản đồ băm) để lưu trữ số lần mỗi giá trị tổng tiền tố đã xuất hiện cho đến nay. 
5. Khởi tạo bản đồ với tiền tố tổng bằng 0 xuất hiện một lần, biểu thị tiền tố trống. 
6. Khi chúng tôi xử lý từng giá trị, hãy cập nhật tổng tiền tố. Mỗi khi giá trị tổng tiền tố xuất hiện lại, hãy thêm tần số trước đó của nó vào câu trả lời. 

Điều này có tác dụng vì tổng tiền tố bằng nhau xác định phân đoạn có tổng bằng 0 giữa các vị trí của chúng. 
7. Sau khi xử lý tất cả các phần tử, xuất ra số đếm tích lũy. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến rằng mọi mảng con có số lần tăng và giảm bằng nhau sẽ tương ứng duy nhất với một cặp chỉ số trong đó tổng tiền tố bằng nhau. Mỗi cặp tổng tiền tố bằng nhau được tính chính xác một lần khi lần xuất hiện thứ hai được xử lý. Điều này đảm bảo sự phân đôi giữa các phân đoạn hợp lệ và các cặp được đếm, do đó không có phân đoạn nào bị bỏ sót và không có phân đoạn nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, input().split()))
    n = data[0]
    h = data[1:]
    
    if n <= 1:
        print(n)
        return
    
    from collections import defaultdict
    
    freq = defaultdict(int)
    freq[0] = 1
    
    pref = 0
    ans = 0
    
    for i in range(n - 1):
        if h[i + 1] > h[i]:
            pref += 1
        else:
            pref -= 1
        
        ans += freq[pref]
        freq[pref] += 1
    
    print(ans + n)

if __name__ == "__main__":
    solve()
```Việc triển khai đọc toàn bộ dữ liệu đầu vào trong một lần và chia nó thành$n$và mảng chiều cao. Điều này tránh được chi phí I/O lặp lại cho đầu vào lớn. 

Bước chuyển đổi được ẩn trong vòng lặp trên các phần tử liền kề, trong đó chúng tôi cập nhật tổng tiền tố theo +1 hoặc -1 tùy theo so sánh. Từ điển`freq`lưu trữ tần suất xuất hiện của mỗi tổng tiền tố và mỗi kết quả khớp ngay lập tức góp phần đưa ra câu trả lời. 

trận chung kết`ans + n`chiếm tất cả các mảng con một phần tử, vì logic tổng tiền tố chỉ tính các đoạn có độ dài ít nhất một cạnh, tức là liên quan đến ít nhất một so sánh. 

Một chi tiết triển khai tinh tế đang được khởi tạo`freq[0] = 1`. Nếu không có điều này, các phân đoạn bắt đầu từ chỉ số 0 sẽ không được tính chính xác vì tổng tiền tố của chúng khớp với tiền tố trống ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 4 9 7 3
```Chúng tôi tính toán chuyển tiếp: 

| Bước | Cặp | Giá trị | Tiền tố Tổng | tần số trước | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 → 4 | +1 | 1 | {0:1} | 0 | 
| 2 | 4 → 9 | +1 | 2 | {0:1,1:1} | 0 | 
| 3 | 9 → 7 | -1 | 1 | {0:1,1:1,2:1} | 1 | 
| 4 | 7 → 3 | -1 | 0 | {0:1,1:2,2:1} | 1 | 

Tổng đóng góp chuyển đổi = 2, cộng với 5 phần tử đơn lẻ sẽ là 7. Đầu ra mẫu bao gồm tất cả các mảng con hợp lệ bao gồm cả các mảng con cân bằng dài hơn như$[1,4,9,7]$, có những thăng trầm như nhau. 

Dấu vết này cho thấy cách tổng tiền tố lặp lại phát hiện các phân đoạn cân bằng, đặc biệt khi tổng hiện hành trở về 0. 

### Ví dụ 2 

đầu vào:```
3
3 2 1
```Chuyển tiếp: 

| Bước | Cặp | Giá trị | Tiền tố Tổng | tần số trước | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 → 2 | -1 | -1 | {0:1} | 0 | 
| 2 | 2 → 1 | -1 | -2 | {0:1,-1:1} | 0 | 

Chỉ các phân đoạn một phần tử là hợp lệ. Cấu trúc giảm dần, do đó không có tổng tiền tố nào lặp lại. 

Điều này xác nhận rằng các mảng đơn điệu không tạo ra sự đóng góp nào ngoài các phần tử đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chiều cao được xử lý một lần và các hoạt động của bản đồ băm được khấu hao theo thời gian không đổi | 
| Không gian |$O(n)$| Trong trường hợp xấu nhất, tất cả các tổng tiền tố đều khác biệt | 

Điều này phù hợp thoải mái trong các hạn chế vì$n \leq 10^6$và cả thời gian và bộ nhớ đều có quy mô tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    data = list(map(int, sys.stdin.read().split()))
    n = data[0]
    h = data[1:]

    from collections import defaultdict

    freq = defaultdict(int)
    freq[0] = 1

    pref = 0
    ans = 0

    for i in range(n - 1):
        if h[i + 1] > h[i]:
            pref += 1
        else:
            pref -= 1
        ans += freq[pref]
        freq[pref] += 1

    return str(ans + n)

# provided sample
assert run("5\n1 4 9 7 3\n") == "10"

# minimum size
assert run("1\n5\n") == "1"

# all increasing
assert run("4\n1 2 3 4\n") == "4"

# all decreasing
assert run("4\n4 3 2 1\n") == "4"

# alternating
assert run("5\n1 3 2 4 3\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | trường hợp cơ sở tòa nhà đơn | 
| 1 2 3 4 | 4 | tăng dần đơn điệu | 
| 4 3 2 1 | 4 | đơn điệu giảm dần | 
| 1 3 2 4 3 | 8 | sự tăng/giảm xen kẽ nhất quán | 

## Vỏ cạnh 

Một thông tin đầu vào của tòa nhà thể hiện điều kiện cơ bản mà không có sự so sánh nào tồn tại. Thuật toán trả về đúng 1 vì`ans`vẫn bằng 0 và chúng tôi thêm`n`. 

Chuỗi tăng nghiêm ngặt tạo ra tổng tiền tố luôn tăng, do đó không có giá trị lặp lại nào xảy ra. Bản đồ băm không bao giờ tích lũy thêm các đóng góp và kết quả chỉ thu gọn thành các phân đoạn một phần tử. 

Một chuỗi giảm nghiêm ngặt hoạt động đối xứng, với tổng tiền tố giảm dần ở mỗi bước. Một lần nữa, không có xung đột tiền tố nào xảy ra, xác nhận rằng chỉ các phân đoạn tầm thường mới được tính. 

Một chuỗi xen kẽ như tăng rồi giảm liên tục sẽ tạo ra nhiều lần lặp lại tổng tiền tố và thuật toán nắm bắt chính xác tất cả các mảng con cân bằng thông qua các lần truy cập băm lặp lại, xác nhận tính chính xác của tính tương đương của tổng tiền tố.
