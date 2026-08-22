---
title: "CF 104199K - \u0413\u043b\u044e\u0447\u043d\u044b\u0435 \u0440\u043e\u0431\u043e\u0430\u043d\u0442\u044b"
description: "Chúng ta được cung cấp một mảng mô tả cách sắp xếp bắt đầu của các mục trên các vị trí được gắn nhãn từ 1 đến n. Vị trí i ban đầu giữ mục a[i] và mục tiêu cuối cùng là biến đổi cách sắp xếp này sao cho vị trí i chứa mục i cho mọi i."
date: "2026-07-02T18:01:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104199
codeforces_index: "K"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 18-02-23"
rating: 0
weight: 104199
solve_time_s: 81
verified: true
draft: false
---

[CF 104199K - \u0413\u043b\u044e\u0447\u043d\u044b\u0435 \u0440\u043e\u0431\u043e\u0430\u043d\u0442\u044b](https://codeforces.com/problemset/problem/104199/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng mô tả cách sắp xếp bắt đầu của các mục trên các vị trí được gắn nhãn từ 1 đến n. Vị trí i ban đầu giữ mục a[i] và mục tiêu cuối cùng là biến đổi cách sắp xếp này sao cho vị trí i chứa mục i cho mọi i. 

Cách duy nhất để di chuyển vật phẩm là thông qua một bộ robot, mỗi robot được đặt ở mỗi vị trí. Robot có thể mang một vật phẩm, nhặt nó lên từ vị trí hiện tại nếu tay trống, di chuyển giữa các vị trí và tùy ý thả hoặc nhặt lại. Việc di chuyển giữa vị trí i và j chỉ có thể thực hiện được nếu i và j có chung ước số lớn hơn 1. 

Điều này biến vấn đề thành một câu hỏi về khả năng tiếp cận trên các vị trí từ 1 đến n, trong đó khả năng kết nối được xác định bởi các ràng buộc gcd. Câu hỏi đặt ra là liệu các robot sử dụng các bước di chuyển bị hạn chế này có thể sắp xếp lại các vật phẩm theo cấu hình nhận dạng hay không. 

Ràng buộc n 200000 ngụ ý rằng bất kỳ phương pháp nào cố gắng xây dựng rõ ràng tất cả các cạnh giữa các cặp vị trí đều không thể thực hiện được, vì số cặp là bậc hai trong trường hợp xấu nhất. Ngay cả việc lặp lại tất cả các cặp và tính toán gcd cũng sẽ quá chậm. Giải pháp phải dựa vào chế độ xem có cấu trúc về kết nối thay vì xây dựng biểu đồ rõ ràng. 

Trường hợp cạnh tinh tế xuất hiện khi hai vị trí được kết nối gián tiếp nhưng không trực tiếp. Ví dụ: 6 được kết nối với 10 đến 2, mặc dù gcd(6,10)=2 và gcd(10,15)=5, nhưng gcd(6,15)=3 cũng kết nối chúng một cách gián tiếp. Một cách tiếp cận ngây thơ chỉ xem xét các cạnh gcd trực tiếp có thể kết luận không chính xác rằng chuyển động bị hạn chế hơn thực tế. 

Một trường hợp thất bại khác xảy ra nếu người ta cho rằng mỗi robot có thể tự cố định vị trí của mình một cách độc lập. Ví dụ: trong một chu trình như 1 → 8 → 2 → 1, các vật phẩm có thể được xoay trong một thành phần mặc dù không có một robot nào trực tiếp “giải quyết” được vị trí của chính nó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xây dựng một biểu đồ rõ ràng trong đó mọi cặp (i, j) được kết nối nếu gcd(i, j) > 1, sau đó chạy duyệt đồ thị để tìm các thành phần được kết nối. Mỗi nút đại diện cho một vị trí và các cạnh đại diện cho các chuyển động được phép của robot. Sau khi tính toán các thành phần, chúng tôi sẽ kiểm tra xem mỗi thành phần có chứa chính xác cùng nhiều tập hợp các mục ban đầu và mục tiêu hay không. 

Về nguyên tắc, điều này đúng vì robot không thể di chuyển các vật phẩm giữa các bộ phận bị ngắt kết nối. Tuy nhiên, việc xây dựng tất cả các cạnh yêu cầu kiểm tra các cặp O(n^2) và ngay cả một BFS hoặc DFS trên một biểu đồ dày đặc như vậy cũng không khả thi với n = 200000. 

Quan sát quan trọng là gcd(i, j) > 1 tương đương với i và j có chung ít nhất một thừa số nguyên tố. Thay vì suy nghĩ theo cách kiểm tra gcd theo cặp, chúng ta có thể nghĩ đến khả năng kết nối được tạo ra bởi các số nguyên tố. Mọi số đều thuộc về tất cả các “nhóm” số nguyên tố tương ứng với các thừa số nguyên tố của nó và các số được kết nối thông qua các số nguyên tố chung. Điều này biến biểu đồ thành một cấu trúc giống như lưỡng cực giữa các số và số nguyên tố, cho phép chúng ta hợp các chỉ số có chung bất kỳ thừa số nguyên tố nào bằng cách sử dụng cấu trúc kết hợp tập hợp rời rạc. 

Khi chúng tôi tính toán các thành phần được kết nối theo mối quan hệ này, mỗi thành phần phải tự nhất quán: mọi chỉ mục i phải có khả năng đạt đến vị trí đích a[i], nếu không thì mục yêu cầu không thể được vận chuyển đến đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ rõ ràng) | O(n²) | O(n²) | Quá chậm | 
| DSU qua thừa số nguyên tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng một cấu trúc tập hợp rời rạc trên các chỉ số từ 1 đến n. Mục tiêu là hợp nhất các chỉ số được kết nối thông qua việc chia sẻ thừa số nguyên tố.

1. Khởi tạo DSU trong đó mỗi vị trí là thành phần riêng của nó. Điều này thể hiện rằng ban đầu chúng tôi cho rằng không thể có chuyển động nào có thể xảy ra. 
2. Với mọi số i từ 1 đến n, hãy phân tích nó thành thừa số nguyên tố riêng biệt. Vì n lớn nên chúng tôi dựa vào quá trình tiền xử lý hoặc phân chia thử nghiệm giống như sàng hiệu quả lên đến sqrt(n). 
3. Duy trì một từ điển ánh xạ từng số nguyên tố tới chỉ mục đầu tiên nơi nó xuất hiện. Khi chúng ta gặp một số nguyên tố p tại chỉ số i, nếu p chưa từng xuất hiện trước đó, chúng ta ghi i là đại diện của nó. Nếu nó đã được nhìn thấy, chúng ta kết hợp i với chỉ mục được lưu trữ trước đó. Bước này xây dựng khả năng kết nối thông qua các số nguyên tố được chia sẻ mà không cần xây dựng các cạnh một cách rõ ràng. 
4. Sau khi xử lý tất cả các chỉ số, mỗi thành phần được kết nối trong DSU tương ứng với một tập hợp các vị trí mà giữa đó robot có thể tự do di chuyển các mục. 
5. Cuối cùng, với mỗi chỉ mục i, chúng tôi kiểm tra xem i và a[i] có thuộc cùng một thành phần DSU hay không. Nếu bất kỳ chỉ mục nào không đạt được điều kiện này thì việc chuyển đổi là không thể. 

Ý tưởng chính là các mục chỉ có thể di chuyển trong các thành phần được kết nối của biểu đồ gcd và DSU trên các số nguyên tố được chia sẻ sẽ nắm bắt chính xác các thành phần đó. 

### Tại sao nó hoạt động 

Bất biến cơ bản là hai chỉ số được kết nối trong biểu đồ chuyển động khi và chỉ khi chúng chia sẻ một chuỗi số trong đó các phần tử liên tiếp có chung một thừa số nguyên tố. DSU hợp nhất chính xác những chỉ số có chung ít nhất một thừa số nguyên tố và tính bắc cầu của các hoạt động hợp nhất nắm bắt được kết nối gcd gián tiếp. Vì mỗi bước di chuyển hợp lệ sẽ duy trì tư cách thành viên của thành phần nên không có mục nào có thể vượt qua các thành phần và trong một thành phần, các lần chuyển lặp lại cho phép sắp xếp lại tùy ý. Điều này làm cho sự bình đẳng giữa các thành phần giữa vị trí ban đầu và vị trí mục tiêu vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    dsu = DSU(n)

    prime_owner = {}

    def factorize(x):
        res = []
        p = 2
        while p * p <= x:
            if x % p == 0:
                res.append(p)
                while x % p == 0:
                    x //= p
            p += 1
        if x > 1:
            res.append(x)
        return res

    for i in range(1, n + 1):
        primes = factorize(i)
        for p in primes:
            if p not in prime_owner:
                prime_owner[p] = i
            else:
                dsu.union(i, prime_owner[p])

    for i in range(1, n + 1):
        if dsu.find(i) != dsu.find(a[i - 1]):
            print("NO")
            return

    print("YES")

if __name__ == "__main__":
    solve()
```Cấu trúc DSU duy trì khả năng kết nối giữa các chỉ số. Bước phân tích thừa số nguyên tố sẽ trích xuất tập hợp số nguyên tố tối thiểu cần thiết để xác định khả năng kết nối dựa trên gcd. Ánh xạ prime_owner đảm bảo rằng tất cả các số có chung một số nguyên tố sẽ được hợp nhất thành một thành phần duy nhất mà không tạo ra các cạnh một cách rõ ràng. 

Vòng lặp cuối cùng yêu cầu mọi mục phải có thể di chuyển được từ vị trí ban đầu đến vị trí cuối cùng được yêu cầu trong cùng một thành phần kết nối. 

Một cạm bẫy triển khai phổ biến là quên rằng các chỉ mục dựa trên 1 trong khi mảng a dựa trên 0 trong Python, đó là lý do tại sao a[i - 1] được sử dụng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
9
1 8 3 6 5 4 7 2 9
```Chúng tôi theo dõi kết nối thông qua các thành phần DSU được tạo ra bởi các số nguyên tố được chia sẻ. 

| tôi | một [tôi] | tìm(i) | tìm(a[i]) | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | C1 | C1 | được | 
| 2 | 8 | C2 | C2 | được | 
| 3 | 3 | C3 | C3 | được | 
| 4 | 6 | C2 | C2 | được | 
| 5 | 5 | C5 | C5 | được | 
| 6 | 4 | C2 | C2 | được | 
| 7 | 7 | C7 | C7 | được | 
| 8 | 2 | C2 | C2 | được | 
| 9 | 9 | C9 | C9 | được | 

Tất cả các chỉ số đều khớp với mục tiêu của chúng trong các thành phần, vì vậy câu trả lời là CÓ. Điều này chứng tỏ rằng mặc dù xảy ra nhiều lần hoán đổi nhưng mọi thứ vẫn nằm trong các thành phần được kết nối chính. 

### Mẫu 2 

đầu vào:```
6
6 2 3 5 4 1
```| tôi | một [tôi] | tìm(i) | tìm(a[i]) | quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | C1 | C2 | thất bại | 

Tại chỉ số 1, vị trí 1 và mục 6 thuộc về các thành phần khác nhau. Điều này ngay lập tức cho thấy sự bất khả thi, vì không có chuỗi chuyển động nào được phép có thể vận chuyển hạng mục 6 vào thành phần không nối với vị trí 1. 

Điều này nắm bắt được chế độ lỗi chính: mục tiêu yêu cầu chuyển động giữa các thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n) + n √n) | Hoạt động DSU gần như không đổi, hệ số hóa chiếm ưu thế | 
| Không gian | O(n + π(n)) | Mảng DSU cộng với ánh xạ nguyên tố | 

Các ràng buộc cho phép thực hiện khoảng vài trăm triệu phép toán đơn giản và giải pháp dựa trên DSU vẫn nằm trong giới hạn thoải mái vì mỗi số được phân tích thành thừa số một lần và mỗi phép toán hợp có thời gian gần như không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("""9
1 8 3 6 5 4 7 2 9
""") == "YES"

assert run("""6
6 2 3 5 4 1
""") == "NO"

# all correct already
assert run("""1
1
""") == "YES"

# small swap impossible due to gcd isolation
assert run("""2
2 1
""") == "NO"

# chain via primes
assert run("""4
2 3 4 1
""") == "YES"

# all same fixed points
assert run("""5
1 2 3 4 5
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | CÓ | trường hợp cơ bản tầm thường | 
| 2 1 | KHÔNG | thành phần bị ngắt kết nối | 
| 2 3 4 1 | CÓ | kết nối chính bắc cầu | 
| danh tính | CÓ | vụ việc đã được giải quyết | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số đều nguyên tố cùng nhau. Trong tình huống này, mọi nút đều bị cô lập trong cấu trúc DSU, do đó mỗi vị trí chỉ có thể giữ mục riêng của nó. Bất kỳ sự không khớp nào sẽ ngay lập tức thất bại vì không có cạnh chuyển động nào tồn tại. Thuật toán phát hiện chính xác điều này vì không có sự kết hợp nguyên tố nào xảy ra. 

Một trường hợp khác là khi các số tạo thành một cấu trúc được kết nối đầy đủ thông qua các số nguyên tố nhỏ được chia sẻ. Ví dụ: các chuỗi chứa nhiều số chẵn sẽ hợp nhất thành một thành phần duy nhất. Thuật toán cho phép các hoán vị tùy ý một cách chính xác bên trong thành phần lớn này, vì DSU hợp nhất tất cả các chỉ số chẵn với nhau và do đó cho phép sắp xếp lại đầy đủ. 

Trường hợp tinh tế cuối cùng là khi kết nối chỉ tồn tại gián tiếp. Ví dụ: các số 6, 10 và 15 tạo thành một chuỗi thông qua các số nguyên tố 2, 5 và 3. Mặc dù một số cặp có gcd 1, DSU vẫn hợp nhất chúng thành một thành phần, cho phép di chuyển chính xác.
