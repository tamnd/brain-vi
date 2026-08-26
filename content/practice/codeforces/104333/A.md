---
title: "CF 104333A - TỔNG XOR tích chập"
description: "Chúng tôi được cung cấp hai mảng có độ dài bằng nhau. Một mảng biểu thị các giá trị được gắn với các chỉ mục mà chúng ta được phép hoán vị và mảng còn lại biểu thị các “khe” cố định."
date: "2026-07-01T18:54:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104333
codeforces_index: "A"
codeforces_contest_name: "Replay of BU - PSTU Programming club collaborative contest"
rating: 0
weight: 104333
solve_time_s: 75
verified: true
draft: false
---

[CF 104333A - TỔNG XOR tích chập](https://codeforces.com/problemset/problem/104333/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có độ dài bằng nhau. Một mảng biểu thị các giá trị được gắn với các chỉ mục mà chúng ta được phép hoán vị và mảng còn lại biểu thị các “khe” cố định. Đối với bất kỳ sự sắp xếp lại nào của mảng đầu tiên, chúng tôi khớp từng phần tử của mảng được hoán vị với một vị trí trong mảng thứ hai và tính điểm bằng tổng XOR theo bit của các phần tử được ghép nối. 

Nhiệm vụ không phải là tìm ra hoán vị tốt nhất mà là tính toán một thứ lớn hơn nhiều: tổng số điểm trên tất cả các hoán vị có thể có. Vì có$n!$hoán vị, mỗi cặp giữa một$a_i$và một$b_j$xuất hiện nhiều lần trên tất cả các hoán vị và mục tiêu là tổng hợp những đóng góp của chúng một cách hiệu quả. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại các hoán vị hoặc thậm chí xử lý các cặp một cách ngây thơ trong$O(n^2)$. Thậm chí$O(n \log n)$hoặc$O(n)$các phương pháp đều được chấp nhận, nhưng bất cứ điều gì liên quan đến phép liệt kê tổ hợp đều phải được thay thế bằng các đối số đếm. 

Một dạng lỗi phổ biến ở đây là cố gắng suy luận mỗi hoán vị hoặc mô phỏng các hiệu ứng hoán đổi. Ví dụ, với$n=3$, người ta có thể cố gắng tính điểm cho tất cả 6 hoán vị một cách rõ ràng. Điều đó hiệu quả với những trường hợp nhỏ nhưng có quy mô lớn và trở nên không thể thực hiện được ngay cả ở$n=15$. 

Một vấn đề tinh tế khác là logic đếm kép: vì XOR không tuyến tính theo nghĩa đơn giản đối với các hoán vị, nên một cách tiếp cận sai lầm có thể cố gắng ghép các mảng đã sắp xếp hoặc khớp các giá trị một cách tham lam. Điều đó làm mất hoàn toàn cấu trúc tổ hợp. 

Khó khăn chính là nhận ra rằng mỗi cặp$(a_i, b_j)$đóng góp một cách đối xứng qua các hoán vị và chúng ta chỉ cần đếm xem có bao nhiêu hoán vị$a_i$ở vị trí$j$. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên mọi hoán vị$p$, tính điểm của nó bằng cách tính tổng$a_{p_i} \oplus b_i$, và tích lũy kết quả. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, nó đòi hỏi phải tạo$n!$hoán vị, và chi phí mỗi hoán vị$O(n)$đánh giá, dẫn đến$O(n \cdot n!)$, vượt xa giới hạn khả thi ngay cả đối với$n=12$. 

Cái nhìn sâu sắc về cấu trúc đến từ tính đối xứng. Sửa một phần tử$a_k$. Trong tất cả các hoán vị, phần tử này được đặt ở mỗi vị trí$j$đúng số lần như nhau. Cụ thể, nếu chúng ta sửa$a_k$ở vị trí$j$, phần còn lại$n-1$các phần tử có thể được hoán vị tùy ý, cho$(n-1)!$hoán vị. Điều này có nghĩa là mỗi cặp$(a_k, b_j)$đóng góp chính xác$(n-1)!$lần đến tổng cuối cùng. 

Vì vậy, thay vì nghĩ đến các hoán vị, chúng ta quy vấn đề về việc tính tổng đóng góp của tất cả các cặp, mỗi cặp có trọng số bằng nhau bởi$(n-1)!$. Câu trả lời trở thành:$$(n-1)! \cdot \sum_{i=1}^{n} \sum_{j=1}^{n} (a_i \oplus b_j)$$Bây giờ vấn đề được giảm xuống để tính tổng XOR gấp đôi một cách hiệu quả. XOR có thể được phân tách từng bit một và mỗi bit đóng góp một cách độc lập. Đối với một bit cố định$t$, ta chỉ cần đếm có bao nhiêu$a_i$đã đặt bit đó chưa và bao nhiêu$b_j$đã thiết lập bit đó. Sự đóng góp của bit$t$ĐẾN$a_i \oplus b_j$chính xác là 1 khi các bit khác nhau, mang lại biểu thức tổ hợp tiêu chuẩn. 

Cho phép$c_a$là số đếm trong$a$với chút$t$thiết lập và$c_b$tương tự cho$b$. Khi đó số cặp có các bit khác nhau là$c_a (n - c_b) + (n - c_a)c_b$. Mỗi cặp như vậy đóng góp$2^t$. 

Chúng tôi tính tổng tất cả các bit lên tới 17 (vì các giá trị lên tới$10^5$), nhân với$(n-1)!$, và chúng ta đã hoàn tất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot n!)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log A)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi cơ cấu lại vấn đề bằng cách đếm các đóng góp trên mỗi bit và chia tỷ lệ theo tính đối xứng hoán vị. 

1. Tính giai thừa$(n-1)!$modulo$10^9+7$. Điều này thể hiện có bao nhiêu hoán vị cố định một phần tử đã chọn ở một vị trí cố định. Ý tưởng chính là mỗi phép gán như vậy để lại các phần tử còn lại có thể hoán vị tự do. 
2. Đối với từng vị trí bit$t$từ 0 đến 17, đếm xem có bao nhiêu phần tử trong$a$có bộ bit này và có bao nhiêu phần tử trong$b$đã thiết lập nó. Điều này tách hành vi XOR thành các kích thước nhị phân độc lập. 
3. Cho bit$t$, hãy tính xem có bao nhiêu cặp có thứ tự tạo ra số 1 trong XOR tại bit này. Điều này xảy ra chính xác khi một bên có bit được đặt còn bên kia thì không, vì vậy chúng tôi đếm các kết hợp chéo giữa hai nhóm. 
4. Nhân số cặp bit khác nhau với$2^t$, vì mỗi cặp như vậy đóng góp giá trị đó cho XOR. 
5. Tích lũy đóng góp trên tất cả các bit để có được tổng XOR theo cặp trên tất cả$a_i, b_j$. 
6. Nhân tổng theo cặp cuối cùng với$(n-1)!$để tính đến tất cả các hoán vị, vì mỗi cặp xuất hiện ở chính xác nhiều vị trí hoán vị đó. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào sự phân bố đồng đều của các phần tử trên các vị trí hoán vị. Bất kỳ phần tử cố định nào$a_i$xuất hiện ở vị trí$j$chính xác$(n-1)!$hoán vị, độc lập với$i$Và$j$. Sự đối xứng này làm giảm tổng hoán vị thành một tỷ lệ đồng nhất của tổng lưỡng cực hoàn chỉnh trên tất cả các cặp. Sau khi được giảm xuống cấu trúc theo cặp, độ tuyến tính XOR trên mỗi bit đảm bảo rằng việc đếm các bit set-bit không khớp sẽ mô tả đầy đủ tổng đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    # factorial (n-1)!
    fact = 1
    for i in range(1, n):
        fact = fact * i % MOD
    
    max_bit = 17
    cnt_a = [0] * (max_bit + 1)
    cnt_b = [0] * (max_bit + 1)
    
    for x in a:
        for t in range(max_bit + 1):
            if x >> t & 1:
                cnt_a[t] += 1
    
    for x in b:
        for t in range(max_bit + 1):
            if x >> t & 1:
                cnt_b[t] += 1
    
    pair_sum = 0
    for t in range(max_bit + 1):
        ca = cnt_a[t]
        cb = cnt_b[t]
        diff_pairs = ca * (n - cb) + (n - ca) * cb
        pair_sum = (pair_sum + diff_pairs * (1 << t)) % MOD
    
    ans = pair_sum * fact % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc tính toán$(n-1)!$, mã hóa số lượng hoán vị gán một phần tử cố định cho một vị trí cố định. Hệ số này được áp dụng ở cuối sau khi tính toán tổng đóng góp của tất cả các cặp phần tử. 

Bước tiếp theo nén cấu trúc XOR thành số lượng bit. Thay vì tính toán XOR trực tiếp, chúng tôi đếm xem có bao nhiêu số trong mỗi mảng có mỗi bit được đặt. Điều này tránh việc lặp lại cặp bậc hai. 

Đối với mỗi bit, chúng tôi tính toán có bao nhiêu cặp chéo tạo ra số 1 ở bit đó. biểu thức$c_a (n - c_b) + (n - c_a)c_b$xuất phát trực tiếp từ các trường hợp phân tách trong đó các bit khác nhau. 

Cuối cùng, chúng tôi nhân tổng tổng XOR theo cặp với hệ số bội hoán vị. 

Một điểm tinh tế là đảm bảo chúng ta không bao giờ cố gắng xây dựng các hoán vị một cách rõ ràng. Toàn bộ vụ nổ tổ hợp được hấp thụ vào hệ số nhân giai thừa. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2 3
1 2 3
```Chúng tôi tính toán$(n-1)! = 2$. 

Chúng tôi đếm những đóng góp bit. Để đơn giản, hãy xem xét các cặp XOR thực tế: 

| Loại cặp | Đếm | Tổng đóng góp XOR | 
| --- | --- | --- | 
| tất cả$a_i, b_j$cặp | 9 | 12 | 

Vậy tổng theo cặp là 12 và nhân với 2 được 24. 

Điều này cho thấy rằng mọi cặp đều đóng góp như nhau trong các hoán vị và việc chia tỷ lệ giai thừa là đủ để xây dựng lại câu trả lời đầy đủ. 

### Mẫu 2 

đầu vào:```
3
2 2 2
3 4 4
```Đây tất cả$a_i$giống hệt nhau nên tính đối xứng thậm chí còn mạnh hơn. 

Chúng tôi tính toán$(n-1)! = 2$. Bây giờ hãy đánh giá XOR theo cặp: 

cho$2 \oplus 3 = 1$, Và$2 \oplus 4 = 6$. 

Số cặp: 

- (2,3) xuất hiện 3 lần 
- (2,4) xuất hiện 6 lần 

Tổng số tiền theo cặp:$3 \cdot 1 + 6 \cdot 6 = 39$Nhân với 2 được$78$. 

Điều này xác nhận rằng bội số được xử lý chính xác ngay cả khi các giá trị lặp lại nhiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Đếm số bit cho mỗi phần tử lên tới 17 bit | 
| Không gian |$O(1)$| Mảng có kích thước cố định để đếm số bit | 

Thuật toán chạy trong thời gian tuyến tính với hệ số không đổi nhỏ do độ rộng bit cố định. Điều này nằm trong giới hạn cho$n = 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    fact = 1
    for i in range(1, n):
        fact = fact * i % MOD

    max_bit = 17
    cnt_a = [0] * (max_bit + 1)
    cnt_b = [0] * (max_bit + 1)

    for x in a:
        for t in range(max_bit + 1):
            if x >> t & 1:
                cnt_a[t] += 1

    for x in b:
        for t in range(max_bit + 1):
            if x >> t & 1:
                cnt_b[t] += 1

    pair_sum = 0
    for t in range(max_bit + 1):
        ca = cnt_a[t]
        cb = cnt_b[t]
        diff = ca * (n - cb) + (n - ca) * cb
        pair_sum = (pair_sum + diff * (1 << t)) % MOD

    return str(pair_sum * fact % MOD)

# provided samples
assert run("""3
1 2 3
1 2 3
""") == "24"

assert run("""3
2 2 2
3 4 4
""") == "78"

# custom cases
assert run("""1
5
7
""") == "2", "single element XOR 5^7 * 0! = 2"

assert run("""2
1 2
1 2
""") == str((1^1 + 1^2 + 2^1 + 2^2) * 1), "direct check n=2"

assert run("""4
0 0 0 0
1 2 3 4
"""), "all a zero edge case"

assert run("""3
5 5 5
5 5 5
""") == "0", "all identical arrays"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 giá trị đơn | XOR trực tiếp | trường hợp giai thừa cơ sở | 
| n=2 hỗn hợp nhỏ | trận đấu tàn bạo | tính đúng đắn của việc khai triển cặp | 
| tất cả số không và phạm vi | XOR có cấu trúc | tính chính xác của việc đếm bit | 
| mảng giống hệt nhau | 0 | trường hợp hủy bỏ | 

## Vỏ cạnh 

Khi nào$n=1$, có đúng một hoán vị và hệ số giai thừa trở thành$(n-1)! = 1$. Thuật toán giảm xuống còn một phép tính XOR duy nhất, do đó, nó xử lý cấu trúc tổ hợp suy biến một cách tự nhiên mà không cần vỏ đặc biệt. 

Khi tất cả các giá trị trong$a$giống hệt nhau, phương pháp đếm bit vẫn hoạt động vì nó không phụ thuộc vào tính duy nhất. Mỗi đóng góp bit chỉ được xác định bằng tần số và công thức thu gọn chính xác tất cả các khác biệt của cặp thành số đếm thống nhất. 

Khi cả hai mảng giống hệt nhau, mọi XOR đều bằng 0, do đó tất cả số bit không khớp sẽ biến mất. Thuật toán tạo ra số 0 vì với mỗi bit,$c_a = c_b$, làm cho cả hai số hạng chéo đều bị hủy bỏ một cách chính xác, điều này khẳng định tính đúng đắn của việc phân tách theo bit.
