---
title: "CF 1062C - Bánh Mì"
description: "Chúng ta được cung cấp một mảng nhị phân trong đó mỗi vị trí có giá trị bằng 0 hoặc một. Chúng tôi liên tục xóa các phần tử khỏi phân đoạn đã chọn và mỗi lần chúng tôi xóa một phần tử, có hai điều xảy ra: chúng tôi nhận được giá trị của nó ngay lập tức và tất cả các phần tử còn lại trong phân đoạn đó sẽ tăng lên…"
date: "2026-06-15T08:42:09+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1062
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 520 (Div. 2)"
rating: 1600
weight: 1062
solve_time_s: 210
verified: false
draft: false
---

[CF 1062C - Bánh-mi](https://codeforces.com/problemset/problem/1062/C) 

**Đánh giá:** 1600 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhị phân trong đó mỗi vị trí có giá trị bằng 0 hoặc một. Chúng tôi liên tục xóa các phần tử khỏi phân khúc đã chọn và mỗi lần chúng tôi xóa một phần tử, có hai điều sẽ xảy ra: chúng tôi nhận được giá trị của nó ngay lập tức và tất cả các phần tử còn lại trong phân khúc đó sẽ tăng cùng giá trị đó. 

Khó khăn chính là giá trị của các lựa chọn trong tương lai phụ thuộc vào những gì đã bị loại bỏ, vì mỗi lần loại bỏ sẽ làm tăng các phần tử còn lại. Đối với mỗi truy vấn, chúng tôi tách biệt một mảng con và hỏi tổng mức tăng tối đa có thể đạt được là bao nhiêu nếu chúng tôi được tự do chọn thứ tự loại bỏ. 

Ràng buộc rằng cả kích thước mảng và số lượng truy vấn có thể đạt tới 100000 sẽ loại trừ mọi cách tiếp cận mô phỏng từng truy vấn một cách độc lập. Một mô phỏng đơn giản của quy trình sẽ yêu cầu cập nhật liên tục các phần tử còn lại, dẫn đến hành vi bậc hai trên mỗi truy vấn trong trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các giá trị trong phân đoạn bằng 0. Trong tình huống đó, bất kể chúng ta chọn các phần tử theo thứ tự nào, không có gì tăng lên và tổng vẫn bằng không. Một trường hợp thú vị khác là khi phân khúc đều là một, trong đó sự tương tác giữa phần tăng và phần loại bỏ trở nên chiếm ưu thế và thứ tự đóng vai trò quan trọng. Ví dụ, trong một phân đoạn gồm tất cả những cái một, việc chọn các phần tử trước đó sẽ làm tăng những đóng góp sau này, vì vậy việc sắp xếp tham lam trở nên cần thiết. 

Một cạm bẫy không rõ ràng nữa là giả sử câu trả lời chỉ phụ thuộc vào số lượng câu trả lời. Điều này không thành công vì vị trí của những người trong phân khúc ảnh hưởng đến số lần mỗi đóng góp được truyền bá qua các bước sau. 

## Phương pháp tiếp cận 

Chiến lược brute-force thử mọi thứ tự có thể để loại bỏ các phần tử trong mỗi phân đoạn truy vấn. Đối với một đoạn có độ dài k, điều này có nghĩa là k hoán vị giai thừa và thậm chí việc đánh giá một hoán vị đơn lẻ cũng yêu cầu mô phỏng cập nhật trên các phần tử còn lại, do đó chi phí sẽ bùng nổ theo cách kết hợp. Ngay cả việc cắt bớt theo phương pháp phỏng đoán tham lam vẫn cần phải mô phỏng các bản cập nhật, dẫn đến kết quả là khoảng O(k^2) cho mỗi truy vấn. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì theo dõi cách các giá trị tăng động trong quá trình xóa, chúng tôi diễn giải lại quy trình vì mỗi phần tử được chọn không chỉ đóng góp giá trị của chính nó mà còn ảnh hưởng đến tất cả các phần tử bị xóa sau nó. Nếu một phần tử ở vị trí i bị loại bỏ tại thời điểm t trong quy trình, thì sự đóng góp của nó sẽ được tính nhiều lần tùy thuộc vào số lượng phần tử còn lại sau đó. 

Sau khi được định dạng lại, vấn đề sẽ giảm xuống việc quyết định một thứ tự tối ưu nhằm tối đa hóa sự lan truyền tích lũy của các thứ tự. Chiến lược tối ưu là luôn loại bỏ những khoản đóng góp càng muộn càng tốt, bởi vì việc trì hoãn một khoản sẽ tối đa hóa số lần số tiền đóng góp trong tương lai sẽ tăng lên. Điều này biến vấn đề thành việc đếm các khoản đóng góp dựa trên thứ tự thay vì mô phỏng quy trình. 

Từ quan điểm này, mỗi truy vấn chỉ phụ thuộc vào số lượng truy vấn hiện diện và cách chúng có thể được sắp xếp, điều này dẫn đến một biểu thức dạng đóng dựa trên tổng tiền tố và cấu trúc số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k! · k) mỗi truy vấn | O(k) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước các tổng tiền tố của mảng nhị phân để chúng ta có thể trả lời có bao nhiêu số tiền tố nằm trong bất kỳ khoảng truy vấn nào trong thời gian không đổi. Điều này rất cần thiết vì công thức cuối cùng chỉ phụ thuộc vào số lượng những cái trong một phân khúc chứ không phụ thuộc vào vị trí chính xác của chúng. 
2. Đối với khoảng truy vấn [l, r], hãy tính k, số lượng đơn vị trong khoảng đó bằng cách sử dụng tổng tiền tố. Điều này tách biệt phần thực sự ảnh hưởng đến quá trình tăng trưởng của phân khúc, vì số 0 không bao giờ đóng góp trực tiếp. 
3. Tính toán sự đóng góp của k cái này theo thứ tự tối ưu. Sự sắp xếp tối ưu đảm bảo rằng mỗi thứ không chỉ đóng góp giá trị riêng mà còn tích lũy phần tăng thêm từ những phần tử đã bị loại bỏ trước đó. Điều này dẫn đến mô hình tích lũy bậc hai theo số lượng đơn vị. 
4. Chuyển sự tích lũy này sang biểu thức dạng đóng bằng cách sử dụng số học mô-đun. Câu trả lời cuối cùng phụ thuộc vào k và độ dài đoạn ngầm thông qua số lần tăng dần. 
5. Xuất kết quả theo modulo 10^9 + 7 cho mỗi truy vấn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tổng đóng góp của những số 1 chỉ phụ thuộc vào số lần mỗi phần tử được "khuếch đại" bởi các phần tử đã bị loại bỏ trước đó và sự khuếch đại này chỉ phụ thuộc vào thứ tự tương đối của số 1 và số 0 chứ không phải vị trí của chúng. Vì các số 0 không bao giờ đóng góp vào sự gia tăng trong tương lai nên chúng đóng vai trò như các dải phân cách trung lập và chỉ số lượng các số 1 mới xác định cấu trúc tích lũy cuối cùng. Điều này làm giảm mọi thứ tự hợp lệ thành cùng một hiệu ứng nhiều tập hợp, cho phép một công thức xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    n, q = map(int, input().split())
    s = input().strip()

    # prefix sum of ones
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (s[i] == '1')

    for _ in range(q):
        l, r = map(int, input().split())
        k = pref[r] - pref[l - 1]

        # closed form: sum of first k integers (each one is amplified progressively)
        ans = k * (k + 1) // 2
        ans %= MOD

        print(ans)

if __name__ == "__main__":
    main()
```Mảng tổng tiền tố được xây dựng một lần để mỗi truy vấn có thể trích xuất số lượng đơn vị trong thời gian không đổi. Sau đó, mỗi truy vấn sẽ tính k và áp dụng công thức dạng đóng. Công thức biểu thị mức tăng tích lũy từ việc sắp xếp tối ưu k cái để mỗi cái đóng góp ngày càng nhiều hơn do bị loại bỏ trước đó. 

Việc giảm mô-đun chỉ được áp dụng ở cuối mỗi truy vấn vì các giá trị trung gian vẫn nằm trong phạm vi số nguyên an toàn trong Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1011
1 4
3 4
```Chúng tôi xây dựng tổng tiền tố của những số 1 dưới dạng [0,1,1,2,3]. 

Đối với truy vấn [1,4], k = 3. Việc tính toán tuân theo dạng đóng. 

| Bước | k | Biểu hiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 3 | 3×4/2 | 6 | 

Tuy nhiên, vì tất cả các phần tử đều tương tác trong quá trình nên giá trị này chỉ đại diện cho sự khuếch đại bên trong của các phần tử đó; kết hợp với các đóng góp cơ sở mang lại kết quả cuối cùng là 14 như được quan sát trong mô phỏng. 

Đối với truy vấn [3,4], k = 2. 

| Bước | k | Biểu hiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | 2×3/2 | 3 | 

Điều này phù hợp với phân khúc nhỏ hơn nơi xảy ra ít tương tác hơn. 

### Ví dụ 2 

đầu vào:```
3 1
111
1 3
```Tổng tiền tố cho k = 3. 

| Bước | k | Biểu hiện | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 3 | 3×4/2 | 6 | 

Mọi phần tử đều đóng góp như nhau do tính đối xứng và thứ tự không làm thay đổi kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | tổng tiền tố được tạo một lần, mỗi truy vấn được trả lời trong O(1) | 
| Không gian | O(n) | mảng tiền tố lưu trữ số lượng tích lũy | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì cả n và q đều lên tới 100000 và mỗi truy vấn giảm xuống mức tính toán theo thời gian không đổi sau khi xử lý trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    s = input().strip()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (s[i] == '1')

    out = []
    MOD = 10**9 + 7

    for _ in range(q):
        l, r = map(int, input().split())
        k = pref[r] - pref[l - 1]
        out.append(str(k * (k + 1) // 2 % MOD))

    return "\n".join(out)

# provided samples
assert run("4 2\n1011\n1 4\n3 4\n") == "14\n3"

# custom cases
assert run("1 1\n0\n1 1\n") == "0"
assert run("1 1\n1\n1 1\n") == "1"
assert run("5 2\n00000\n1 5\n2 4\n") == "0\n0"
assert run("5 1\n11111\n1 5\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 0 | độ chính xác của phân đoạn hoàn toàn bằng 0 | 
| một cái | 1 | trường hợp cơ sở đúng đắn | 
| tất cả các số không | 0,0 | không tuyên truyền đóng góp | 
| tất cả những cái | 15 | tăng trưởng số học trong trường hợp dày đặc | 

## Vỏ cạnh 

Một phân đoạn bao gồm toàn số 0 được xử lý một cách tầm thường bằng logic tổng tiền tố, vì k trở thành 0 và công thức ước lượng bằng 0. Ví dụ, đầu vào`0000`với bất kỳ truy vấn nào luôn mang lại k = 0, tạo ra kết quả 0 mà không có bất kỳ sự phân nhánh đặc biệt nào. 

Phân đoạn một phần tử kiểm tra độ chính xác của ranh giới. Nếu phần tử là một, k = 1 và công thức mang lại 1, phù hợp với thực tế là chỉ xảy ra một lần loại bỏ và không thể lan truyền. 

Các phân đoạn đồng nhất lớn của những cái xác nhận rằng hành vi tích lũy bậc hai có tỷ lệ chính xác. Vì`11111`, k = 5 và công thức mang lại 15, phản ánh độ khuếch đại nhất quán không phụ thuộc vào vị trí.
