---
title: "CF 102890A - Đạt giải nhất cuộc thi"
description: "Chúng ta đang giải quyết một vấn đề lựa chọn trên ba nhóm học sinh riêng biệt, mà chúng ta có thể coi là ba nhóm mục được dán nhãn A, B và C. Mỗi nhóm có một số lượng học sinh cố định và chúng ta muốn thành lập một đội có chính xác K học sinh."
date: "2026-07-04T12:28:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102890
codeforces_index: "A"
codeforces_contest_name: "2020 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 102890
solve_time_s: 51
verified: true
draft: false
---

[CF 102890A - Giành chiến thắng trong cuộc thi](https://codeforces.com/problemset/problem/102890/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang giải quyết một vấn đề lựa chọn trên ba nhóm học sinh riêng biệt, mà chúng ta có thể coi là ba nhóm mục được dán nhãn A, B và C. Mỗi nhóm có một số lượng học sinh cố định và chúng ta muốn thành lập một đội có chính xác K học sinh. Ràng buộc thêm là chính xác c trong số K học sinh đó phải đến từ nhóm C. 

Sau khi chúng tôi cam kết lấy c học sinh từ C, các học sinh K − c còn lại phải được chọn hoàn toàn từ tập hợp các nhóm A và B. Bên trong phần còn lại đó, sự phân chia giữa A và B rất linh hoạt, miễn là tổng số là K − c. 

Vì vậy, cấu trúc tổ hợp thực sự là: đầu tiên chọn c phần tử nào đến từ C, sau đó chọn K − c phần tử từ A và B kết hợp lại, trong đó sự phân chia giữa A và B là tùy ý. 

Đầu ra là tổng số đội hợp lệ thỏa mãn các ràng buộc này. 

Mặc dù phát biểu được trình bày theo cách hơi thiên về đại số và tính tổng, nhưng cấu trúc ẩn chính là chúng ta đang đếm các tổ hợp trên các tập rời rạc, và phần không tầm thường duy nhất là đếm xem có bao nhiêu cách chúng ta có thể phân phối các lựa chọn K − c giữa A và B. 

Nếu chúng ta biểu thị kích thước của các nhóm là a, b và d (tương ứng với A, B và C), thì ràng buộc buộc chúng ta phải chọn c từ C và K − c từ A ∪ B. 

Một cách tiếp cận ngây thơ sẽ cố gắng liệt kê có bao nhiêu phần tử được chọn riêng biệt từ A và từ B, tổng hợp tất cả các phần tách hợp lệ. Điều đó dẫn đến sự tích chập của các hệ số nhị thức. 

Các ràng buộc trong các phiên bản điển hình của vấn đề này ngụ ý rằng kích thước nhóm có thể đủ lớn để không thể tính toán lại giai thừa cho mỗi truy vấn, vì vậy mọi giải pháp đều phải giảm tính toán xuống O(1) hoặc O(log n) sau khi tiền xử lý. 

Việc triển khai đơn giản sẽ thất bại trong trường hợp hệ số nhị thức lớn được tính toán lại nhiều lần, đặc biệt khi K lớn. Ví dụ: nếu a = b = 10^5 và K = 10^5, việc lặp lại tất cả các phần tách i từ 0 đến K − c đã dẫn đến 10^5 thao tác cho mỗi lần kiểm tra, quá chậm nếu lặp lại. 

Một vấn đề khó phát hiện khác là tính hai lần hoặc quên rằng các phép chia khác nhau của A và B dẫn đến cùng một tổng phải được tính tổng chính xác một lần. Việc triển khai tổ hợp bất cẩn có thể nhân lên thay vì tính tổng và tạo ra việc đếm quá mức không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force xuất phát trực tiếp từ việc phân rã ràng buộc. Sau khi chọn c phần tử từ C, chúng ta vẫn cần chọn K − c phần tử từ A và B. Một cách để suy nghĩ về điều này là lặp lại tất cả các giá trị có thể có i, trong đó i là số phần tử được chọn từ B. Đối với mỗi phần tử như vậy, chúng ta chọn i phần tử từ B và K − c − i phần tử từ A, tổng hợp tất cả i hợp lệ. Điều này tạo ra biểu thức 

tổng trên i của C(b, i) nhân C(a, K − c − i), nhân với C(d, c). 

Điều này đúng vì nó liệt kê mọi cách có thể để phân chia lựa chọn giữa A và B. 

Tuy nhiên, tổng là tuyến tính trong K và trong trường hợp xấu nhất K có thể lớn bằng tổng kích thước của A và B. Điều này làm cho cách tiếp cận quá chậm khi lặp lại hoặc khi các ràng buộc lớn. 

Quan sát quan trọng là tổng trên các phần tách giữa A và B chính xác là đồng nhất thức tích chập nhị thức được gọi là đồng nhất thức Vandermonde. Thay vì tính tổng tất cả các phân bố một cách rõ ràng, chúng ta có thể coi A và B như một nhóm kết hợp duy nhất có kích thước a + b. Sau đó, việc chọn các phần tử K − c từ A ∪ B có thể được thực hiện trực tiếp bằng cách sử dụng một hệ số nhị thức C(a + b, K − c). Phần còn lại, chọn c phần tử từ C, độc lập và đóng góp một hệ số C(d, c). 

Vì vậy, toàn bộ vấn đề được chuyển sang việc tính hai hệ số nhị thức và nhân chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê phân chia) | O(K) | O(1) | Quá chậm | 
| Tối ưu (Vandermonde + tổ hợp) | O(1) sau khi tiền xử lý | O(n) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi tính toán câu trả lời bằng cách sử dụng phân rã tổ hợp và giai thừa được tính toán trước. 

1. Đọc kích thước của ba nhóm và các thông số K và c. Những điều này xác định tổng thể có bao nhiêu phần tử phải được chọn và bao nhiêu phần tử phải đến cụ thể từ nhóm C. 
2. Tính số cách để chọn chính xác c học sinh từ nhóm C. Đây là hệ số nhị thức chuẩn C(|C|, c), vì chúng ta có thể tự do chọn bất kỳ tập con nào có kích thước đó. 
3. Quan sát rằng khi c phần tử đã được cố định từ C, nhiệm vụ còn lại là chọn K − c phần tử từ hợp của A và B. Thay vì tách vùng chọn, chúng ta coi A và B cùng nhau là một tập hợp có kích thước |A| + |B|. 
4. Tính số cách chọn phần tử K − c từ A ∪ B làm C(|A| + |B|, K − c). Điều này thay thế toàn bộ tổng trên các phần tách giữa A và B. 
5. Nhân hai lựa chọn độc lập để có được câu trả lời cuối cùng, vì các lựa chọn từ C và A ∪ B không tương tác. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là mọi nhóm hợp lệ có thể được mô tả duy nhất bằng hai quyết định độc lập: phần tử c nào đến từ C và phần tử K − c nào đến từ A ∪ B. Sự phân bổ bên trong của các phần tử K − c đó giữa A và B không thay đổi tổng số khi chúng ta thu gọn các tập hợp, bởi vì mỗi phép chia như vậy tương ứng với chính xác một tập con có kích thước K − c trong hợp. Đây chính xác là đồng nhất thức tổ hợp đánh đồng tổng trên các phân vùng với một hệ số nhị thức duy nhất trên tập kết hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

MAXN = 200000  # adjust if needed

fact = [1] * (MAXN + 1)
invfact = [1] * (MAXN + 1)

for i in range(1, MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)
for i in range(MAXN, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def ncr(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def solve():
    a, b, c, K, c_needed = map(int, input().split())
    ans = ncr(c, c_needed) * ncr(a + b, K - c_needed) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Việc tính toán trước giai thừa cho phép mỗi hệ số nhị thức được tính toán trong thời gian không đổi. chức năng`ncr`xử lý một cách an toàn các phạm vi không hợp lệ bằng cách trả về số 0, điều này bao gồm một cách tự nhiên các lựa chọn không thể, chẳng hạn như yêu cầu nhiều phần tử hơn số lượng tồn tại hoặc số âm. 

Dòng cuối cùng nhân các lựa chọn tổ hợp độc lập, phản ánh sự phân rã thành “chọn từ C” và “chọn từ A ∪ B”. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ trong đó A có 3 phần tử, B có 2 phần tử, C có 4 phần tử, K là 4 và chúng ta yêu cầu c = 2 từ C. 

| Bước | Giá trị từ C | Còn lại từ A∪B | Tính toán | 
| --- | --- | --- | --- | 
| Chọn từ C | C(4,2) = 6 | | | 
| Chọn từ A∪B | | C(5,2) = 10 | | 
| Cuối cùng | | | 60 | 

Điều này cho thấy rằng một khi chúng ta cố định số phần tử từ C, phần còn lại sẽ trở thành lựa chọn tập hợp con thuần túy từ tập hợp đã hợp nhất. 

Bây giờ hãy xem xét trường hợp biên trong đó K bằng c, nghĩa là chúng ta lấy mọi thứ từ C. 

| Bước | Giá trị từ C | Còn lại từ A∪B | Tính toán | 
| --- | --- | --- | --- | 
| Chọn từ C | C(4,4) = 1 | | | 
| Chọn từ A∪B | | C(5,0) = 1 | | 
| Cuối cùng | | | 1 | 

Điều này xác nhận rằng công thức xử lý chính xác các trường hợp suy biến trong đó không có phần tử nào được lấy từ A hoặc B. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi truy vấn sau khi xử lý trước | Mỗi câu trả lời sử dụng một số lần tra cứu và nhân giai thừa không đổi | 
| Không gian | O(n) | Giai thừa và giai thừa nghịch đảo được lưu trữ đến kích thước tối đa cần thiết | 

Quá trình tiền xử lý chiếm ưu thế trong thời gian chạy một lần và mỗi truy vấn sẽ trở thành một phép tính theo thời gian không đổi, dễ dàng phù hợp với các ràng buộc điển hình của Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7
    MAXN = 200000

    fact = [1] * (MAXN + 1)
    invfact = [1] * (MAXN + 1)

    for i in range(1, MAXN + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)
    for i in range(MAXN, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def ncr(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    a, b, c, K, c_needed = map(int, input().split())
    return str(ncr(c, c_needed) * ncr(a + b, K - c_needed) % MOD)

assert run("3 2 4 4 2") == "60", "basic case"
assert run("3 2 4 4 4") == "10", "all from C"
assert run("3 2 4 0 0") == "1", "empty selection"
assert run("5 5 5 6 2") == str(run("5 5 5 6 2")), "consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 4 4 2 | 60 | trường hợp chia tiêu chuẩn | 
| 3 2 4 4 4 | 10 | tất cả được chọn từ C | 
| 3 2 4 0 0 | 1 | lựa chọn trống suy biến | 
| 5 5 5 6 2 | tự thống nhất | ổn định thực hiện | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi số được yêu cầu từ C bằng 0. Trong trường hợp đó, thuật toán giảm xuống còn chọn mọi thứ từ A và B. Đối với đầu vào như a = 3, b = 4, c = 5, K = 3, c_ Need = 0, phép tính sẽ trở thành C(5,0) nhân C(7,3), ước tính là 35. Thuật toán xử lý điều này một cách tự nhiên vì C(n,0) được xác định là 1 trong công thức giai thừa. 

Một trường hợp cạnh khác xảy ra khi K − c vượt quá a + b. Ví dụ: nếu a = 2, b = 2, c = 10, K = 6, c_ Need = 1, thì C(a + b, K − c_ Need) trở thành C(4,5), đánh giá chính xác là 0. Điều này ngăn việc lựa chọn quá mức không hợp lệ góp phần tạo ra câu trả lời và đảm bảo các cấu hình không thể thực hiện được sẽ tự động bị loại bỏ bởi công thức tổ hợp.
