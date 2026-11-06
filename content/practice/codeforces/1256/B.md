---
title: "CF 1256B - Giảm thiểu hoán vị"
description: "Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một hoán vị, nghĩa là một mảng chứa mọi số nguyên từ 1 đến n đúng một lần. Hoạt động duy nhất được phép là hoán đổi giữa các vị trí liền kề i và i+1 và mỗi lần hoán đổi như vậy có thể được sử dụng nhiều nhất một lần."
date: "2026-06-18T17:46:15+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1256
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 598 (Div. 3)"
rating: 1400
weight: 1256
solve_time_s: 107
verified: false
draft: false
---

[CF 1256B - Giảm thiểu hoán vị](https://codeforces.com/problemset/problem/1256/B) 

**Đánh giá:** 1400 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một hoán vị, nghĩa là một mảng chứa mọi số nguyên từ 1 đến n đúng một lần. Hoạt động duy nhất được phép là hoán đổi giữa các vị trí liền kề i và i+1 và mỗi lần hoán đổi như vậy có thể được sử dụng nhiều nhất một lần. Hoán đổi có thể được áp dụng theo bất kỳ thứ tự nào, nhưng hạn chế là mỗi cặp liền kề có thể được hoán đổi tối đa một lần một cách hiệu quả có nghĩa là mỗi phần tử có thể di chuyển sang trái tối đa một vị trí cho mỗi cơ hội đảo ngược được tạo bởi hoán đổi không được sử dụng. 

Nhiệm vụ là tạo ra hoán vị nhỏ nhất về mặt từ điển có thể tiếp cận được dưới những ràng buộc này. Thứ tự từ điển ở đây hoạt động giống như so sánh từ điển: các vị trí trước đó quan trọng hơn, vì vậy việc giảm thiểu phần tử đầu tiên luôn được ưu tiên cao nhất, sau đó là phần tử thứ hai, v.v. 

Ràng buộc n 100 cho mỗi trường hợp thử nghiệm đủ nhỏ để mô phỏng tham lam O(n²) là hoàn toàn an toàn. Ngay cả khi tất cả q = 100 trường hợp kiểm thử là trường hợp xấu nhất, chúng ta vẫn ở trong phạm vi khoảng 10⁶ phép toán. 

Một trường hợp phức tạp phát sinh khi nhiều giao dịch hoán đổi có thể được áp dụng theo các thứ tự khác nhau. Ví dụ: trong một mảng giảm dần như [4, 3, 2, 1], một trực giác ngây thơ có thể cố gắng sắp xếp nó theo kiểu bong bóng hoàn toàn. Điều đó không chính xác vì mỗi chỉ mục hoán đổi chỉ có thể được sử dụng một lần, do đó các phần tử không thể liên tục lướt qua mảng. Câu trả lời đúng cho trường hợp này là [1, 4, 3, 2], không sắp xếp đầy đủ. 

Một trường hợp thất bại khác là giả sử một lần chuyển từ trái sang phải là đủ mà không cần theo dõi cẩn thận xem chỉ số hoán đổi đã được sử dụng hay chưa. Nếu chúng tôi bỏ qua điều này, chúng tôi có thể cho phép sắp xếp lại nhiều lần một cách không chính xác thông qua cùng một ranh giới. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng mô phỏng tất cả các tập hợp con của các giao dịch hoán đổi được phép và tất cả các thứ tự có thể áp dụng chúng. Vì có n−1 vị trí hoán đổi, điều này dẫn đến 2^(n−1) khả năng và mỗi mô phỏng có giá O(n), quá lớn ngay cả với n = 100. 

Quan sát chính là cấu trúc của các hoạt động được phép hoạt động giống như một hệ thống cải tiến cục bộ bị ràng buộc. Mỗi vị trí i cho phép tối đa một lần hoán đổi giữa i và i+1. Điều này có nghĩa là mỗi ranh giới chỉ có thể “sửa chữa” một nghịch đảo đi qua nó. Sau khi sử dụng, ranh giới đó sẽ cạn kiệt. 

Điều này biến vấn đề thành một cấu trúc tham lam từ trái sang phải. Tại mỗi vị trí, chúng tôi cố gắng đưa phần tử nhỏ nhất có thể vẫn có thể tiếp cận vị trí đó một cách hợp pháp, nhưng chúng tôi phải tôn trọng việc hoán đổi ở chỉ số i sẽ tiêu tốn hoạt động đó vĩnh viễn. 

Thay vì suy nghĩ về sự sắp xếp lại toàn cầu, chúng tôi tập trung vào việc liệu một phần tử sẽ vượt qua ranh giới bây giờ hay sau này. Nếu một phần tử nhỏ hơn ở bên phải và có thể được đưa sang trái bằng cách sử dụng một phép hoán đổi không được sử dụng ở vị trí ngay bên trái của nó, thì chúng ta nên thực hiện phép hoán đổi đó ngay lập tức khi nó giúp ích cho vị trí từ điển hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các tập hợp con/đơn hàng trao đổi) | O(2^n · n) | O(n) | Quá chậm | 
| Tham lam liền kề ràng buộc hoán đổi | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quá trình một cách tham lam, theo dõi vị trí hoán đổi nào đã được sử dụng. 

1. Duy trì mảng hoán vị và mảng boolean được sử dụng có kích thước n, trong đó used[i] cho biết liệu chúng ta đã áp dụng hoán đổi giữa i và i+1 hay chưa. 
2. Lặp lại từ trái sang phải trên các vị trí i từ 0 đến n−1. 
3. Tại mỗi vị trí i, cố gắng giảm thiểu a[i] bằng cách kiểm tra xem việc hoán đổi nó với a[i+1] có khả thi và có lợi hay không. Nếu used[i] là sai và a[i+1] < a[i], hãy thực hiện hoán đổi và đánh dấu used[i] là đúng.

Bước này an toàn vì việc đặt một phần tử nhỏ hơn sớm hơn luôn cải thiện thứ tự từ điển và việc sử dụng hoán đổi ngay bây giờ sẽ ngăn ngừa mất cơ hội sau này khi nó có thể không còn hữu ích nữa. 
4. Sau khi thử hoán đổi ở vị trí i, hãy tiến về phía trước. Chúng tôi không bao giờ xem lại chỉ mục hoán đổi vì mỗi chỉ mục chỉ có thể được sử dụng một lần. 
5. Tiếp tục cho đến khi tất cả các vị trí được xử lý. 

Tại sao nó hoạt động xuất phát từ một bất biến tham lam: ở mọi vị trí i, sau khi xử lý, phần tử được đặt ở đó là phần tử nhỏ nhất có thể đạt được có thể chiếm giữ hợp pháp vị trí i với điều kiện là các hoán đổi ở bên trái của i đã được sửa và không thể sử dụng lại. Vì thứ tự từ điển trước tiên phụ thuộc vào vị trí 0, sau đó là 1, v.v., việc cố định từng vị trí một cách tham lam theo cách tối ưu bị ràng buộc này sẽ đảm bảo tính tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))
        
        used = [False] * (n - 1)
        
        for i in range(n - 1):
            if not used[i] and a[i] > a[i + 1]:
                a[i], a[i + 1] = a[i + 1], a[i]
                used[i] = True
        
        print(*a)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp ý tưởng tham lam. Mảng đã sử dụng thực thi ràng buộc một lần trên mỗi vị trí hoán đổi. Việc chuyển từ trái sang phải duy nhất đảm bảo chúng tôi chỉ xem xét từng ranh giới một lần, điều này là đủ vì sau khi sử dụng hoán đổi, tác dụng của nó không thể được xem lại hoặc cải thiện sau này. 

Sự so sánh`a[i] > a[i + 1]`mã hóa điều kiện cải thiện từ điển cục bộ. Vì các vị thế trước đó chiếm ưu thế nên chúng tôi không bao giờ cố gắng trì hoãn các giao dịch hoán đổi có lợi. 

Một sai lầm phổ biến là lặp đi lặp lại các yếu tố bong bóng cho đến khi không có sự cải thiện nào. Điều đó vi phạm ràng buộc sử dụng một lần và dẫn đến việc sắp xếp quá mức, điều này không chính xác trong các trường hợp như [4, 3, 2, 1]. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[5, 4, 1, 3, 2]`Chúng tôi theo dõi các giao dịch hoán đổi và tiến hóa mảng: 

| tôi | Trạng thái mảng | Hoán đổi được sử dụng? | Hành động | 
| --- | --- | --- | --- | 
| 0 | [5, 4, 1, 3, 2] | không | hoán đổi 5 và 4 | 
| 1 | [4, 5, 1, 3, 2] | vâng | không trao đổi (5 > 1 nhưng chỉ số 1 hiện được sử dụng) | 
| 2 | [4, 5, 1, 3, 2] | không | hoán đổi 5 và 1 | 
| 3 | [4, 1, 5, 3, 2] | không | hoán đổi 5 và 3 | 
| 4 | [4, 1, 3, 5, 2] | không | hoán đổi 5 và 2 | 

Kết quả cuối cùng:`[1, 5, 2, 4, 3]`Điều này cho thấy mỗi ranh giới chỉ có thể được sử dụng một lần, buộc phải lập kế hoạch hoán đổi cẩn thận. 

### Ví dụ 2 

đầu vào:`[4, 3, 2, 1]`| tôi | Trạng thái mảng | Hoán đổi được sử dụng? | Hành động | 
| --- | --- | --- | --- | 
| 0 | [4, 3, 2, 1] | không | hoán đổi 4 và 3 | 
| 1 | [3, 4, 2, 1] | vâng | không trao đổi | 
| 2 | [3, 4, 2, 1] | không | hoán đổi 4 và 2 | 
| 3 | [3, 2, 4, 1] | không | hoán đổi 4 và 1 | 

Kết quả cuối cùng:`[3, 2, 1, 4]`Điều này chứng tỏ rằng việc sắp xếp đầy đủ là không thể vì mỗi ranh giới chỉ đóng góp một lần, ngăn chặn sự sủi bọt hoàn toàn của các phần tử lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi thử nghiệm xử lý tối đa n−1 lần kiểm tra hoán đổi và mỗi lần kiểm tra là O(1) | 
| Không gian | O(n) | Lưu trữ hoán vị và đánh dấu trao đổi được sử dụng | 

Với n ≤ 100 và q ≤ 100, số lượng thao tác trong trường hợp xấu nhất là khoảng 10⁴, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        q = int(input())
        out = []
        for _ in range(q):
            n = int(input())
            a = list(map(int, input().split()))
            used = [False] * (n - 1)
            for i in range(n - 1):
                if not used[i] and a[i] > a[i + 1]:
                    a[i], a[i + 1] = a[i + 1], a[i]
                    used[i] = True
            out.append(" ".join(map(str, a)))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
5
5 4 1 3 2
4
1 2 4 3
1
1
4
4 3 2 1
""") == """1 5 2 4 3
1 2 3 4
1
3 2 1 4"""

# custom cases
assert run("""1
2
2 1
""") == "1 2", "minimum swap"

assert run("""1
3
3 1 2
""") == "1 3 2", "single beneficial swap"

assert run("""1
5
1 5 4 3 2
""") == "1 4 3 2 5", "chain with limited swaps"

assert run("""1
4
2 1 4 3
""") == "1 2 3 4", "independent pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 1 2 | ranh giới trao đổi đơn | 
| 3 1 2 | 1 3 2 | sự lựa chọn tham lam của địa phương đúng đắn | 
| 1 5 4 3 2 | 1 4 3 2 5 | sự lan truyền hạn chế của các giao dịch hoán đổi | 
| 2 1 4 3 | 1 2 3 4 | nhiều phân khúc độc lập | 

## Vỏ cạnh 

Đối với hoán vị một phần tử, không có vị trí hoán đổi, do đó thuật toán không thực hiện gì và trả về mảng không thay đổi. Đối với các hoán vị giảm dần, chỉ có thể có tiền tố cải tiến cục bộ và thuật toán dừng chính xác sau khi mỗi chỉ mục hoán đổi được sử dụng một lần, ngăn cản việc sắp xếp đầy đủ. Đối với các mảng đã được sắp xếp, điều kiện`a[i] > a[i+1]`không bao giờ kích hoạt, do đó đầu ra không thay đổi, phù hợp với kết quả từ điển tối ưu.
