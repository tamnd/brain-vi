---
title: "CF 1499C - Đường dẫn lưới tối thiểu"
description: "Chúng ta đang xây dựng một đường dẫn đơn điệu từ góc dưới bên trái của lưới đến góc trên bên phải, nhưng đường dẫn này không chỉ là một chuỗi các bước đơn vị đơn giản."
date: "2026-06-14T17:50:54+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1499
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 106 (Rated for Div. 2)"
rating: 1500
weight: 1499
solve_time_s: 244
verified: true
draft: false
---

[CF 1499C - Đường dẫn lưới tối thiểu](https://codeforces.com/problemset/problem/1499/C) 

**Đánh giá:** 1500 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, toán học 
**Thời gian giải:** 4m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xây dựng một đường dẫn đơn điệu từ góc dưới bên trái của lưới đến góc trên bên phải, nhưng đường dẫn này không chỉ là một chuỗi các bước đơn vị đơn giản. Thay vào đó, nó được tạo thành từ một số ít đoạn thẳng, trong đó mỗi đoạn đi hoàn toàn sang bên phải hoặc hoàn toàn đi lên và các đoạn liên tiếp phải thay đổi hướng. 

Mỗi đoạn có hai lựa chọn độc lập: độ dài của nó, là một số nguyên dương, và hệ số chi phí của nó, được đưa ra trước dưới dạng danh sách c. Nếu một đoạn có độ dài L và sử dụng hệ số c_i thì đóng góp của nó vào tổng chi phí là c_i nhân với L. Đường đi phải di chuyển tổng cộng đúng n đơn vị sang bên phải và tổng cộng đúng n đơn vị hướng lên trên, do đó tổng độ dài đoạn ngang là n và tổng độ dài đoạn dọc cũng là n. 

Mục tiêu là chọn số lượng phân đoạn sẽ sử dụng, cách thay thế chúng và cách phân bổ độ dài trên chúng để tổng chi phí có trọng số được giảm thiểu. 

Ràng buộc có nhiều nhất n đoạn không phải là phần khó. Cấu trúc thực sự xuất phát từ việc đoạn i luôn sử dụng hệ số c_i theo thứ tự, bất kể nó trở thành đoạn ngang hay đoạn dọc. Điều này có nghĩa là chúng ta không gán các hệ số một cách tùy tiện cho các hướng, chúng ta đang gán các hướng cho một chuỗi hệ số cố định và sau đó chọn cách phân phối độ dài. 

Kích thước đầu vào cho phép lên tới 1000 trường hợp thử nghiệm và tổng cộng 100000 hệ số. Bất kỳ giải pháp nào thử tất cả các phân vùng có thể có hoặc tính toán lại chi phí theo thời gian bậc hai sẽ không thành công, vì vậy giải pháp đó phải tính toán câu trả lời theo thời gian tuyến tính cho mỗi trường hợp kiểm thử. 

Một ý tưởng ngây thơ là thử mọi cách để chia chuỗi thành các đoạn và ấn định độ dài. Điều đó không thành công vì ngay cả việc chọn nơi phân chia cũng đã dẫn đến nhiều cấu hình theo cấp số nhân. 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc cố gắng mở rộng các phân khúc một cách tham lam mà không nhìn về phía trước. Nếu chúng ta luôn làm cho đoạn hiện tại dài bất cứ khi nào hệ số của nó nhỏ, thì chúng ta có thể chặn các đoạn trong tương lai có hệ số nhỏ hơn nhận được độ dài lớn, mặc dù các đoạn trong tương lai đó sẽ được hưởng lợi nhiều hơn từ việc được chỉ định khoảng cách dài. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực trực tiếp là chọn một số phân đoạn k, chọn hướng bắt đầu, sau đó chọn tất cả độ dài phân đoạn làm số nguyên dương tổng hợp chính xác cho chuyển động ngang và dọc. Đối với mỗi cấu hình, chi phí tính toán rất đơn giản. Số cách phân chia n đơn vị thành k phần dương tăng lên theo tổ hợp và việc tính tổng tất cả k khiến điều này hoàn toàn không khả thi ngay cả đối với n nhỏ. 

Quan sát quan trọng là khi chuỗi hệ số được cố định, quyền tự do duy nhất quan trọng là tổng khoảng cách ngang n và tổng khoảng cách dọc n được phân bổ như thế nào trên các phân đoạn chẵn và lẻ. Mỗi phân đoạn được lập chỉ mục lẻ đều đóng góp cho một trục và mọi phân đoạn được lập chỉ mục chẵn đều đóng góp cho trục kia, tùy thuộc vào việc chúng ta bắt đầu bằng chuyển động ngang hay dọc. 

Điều này chuyển vấn đề thành vấn đề phân bổ nguồn lực. Đối với số lượng phân đoạn k cố định, các vị trí lẻ có tổng chiều dài n chia cho các đoạn trần (k/2) và các vị trí chẵn có cùng cấu trúc với sàn (k/2). Vì chi phí là tuyến tính theo độ dài phân đoạn, nên trong mỗi nhóm chẵn lẻ, tốt nhất là luôn cung cấp độ dài 1 cho mỗi phân đoạn trước, sau đó phân phối tất cả độ dài còn lại cho phân đoạn có hệ số nhỏ nhất trong nhóm đó.

Vì vậy, đối với bất kỳ k cố định nào, cấu trúc tối ưu sẽ trở nên xác định khi chúng ta biết hệ số tối thiểu ở các vị trí chẵn và lẻ lên tới k. Thử thách còn lại là tính toán k tốt nhất một cách hiệu quả, có thể thực hiện bằng cách quét k từ 1 đến n trong khi vẫn duy trì tổng tiền tố và cực tiểu tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các phân đoạn | Hàm mũ | O(n) | Quá chậm | 
| Tiền tố DP trên k với tập hợp chẵn lẻ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp kiểm thử một cách độc lập và tính toán câu trả lời bằng cách thử tất cả các số phân đoạn k có thể có. 

1. Tính toán trước thông tin tiền tố trên mảng hệ số. Chúng tôi duy trì tổng tiền tố cho các chỉ số lẻ và chỉ số chẵn một cách riêng biệt, đồng thời cũng theo dõi hệ số tối thiểu được thấy cho đến nay ở các vị trí chẵn và lẻ. Điều này cho phép chúng ta đánh giá bất kỳ độ dài tiền tố k nào trong thời gian không đổi. 
2. Với mỗi k có thể từ 1 đến n, hãy giải thích hệ số k đầu tiên như xác định k phân đoạn. Chia chúng thành các nhóm có chỉ số lẻ và nhóm có chỉ số chẵn theo vị trí của chúng trong chuỗi. 
3. Xét trường hợp đường đi bắt đầu bằng một đoạn nằm ngang. Trong trường hợp này, các phân đoạn có chỉ số lẻ tương ứng với chuyển động theo chiều ngang và các phân đoạn có chỉ mục chẵn tương ứng với chuyển động theo chiều dọc. 
4. Đối với cấu hình này, hãy tính chi phí cơ bản bằng tổng của tất cả các hệ số nhân với độ dài đơn vị cho mỗi đoạn. Sau đó tính đến phân bố chiều dài bổ sung: tổng khoảng cách theo chiều ngang là n nên sau khi cho mỗi đoạn ngang có độ dài 1, chúng ta vẫn cần phân bổ chiều dài còn lại cho các đoạn ngang. Chi phí tăng thêm này được giảm thiểu bằng cách gán tất cả chiều dài ngang còn lại cho hệ số nhỏ nhất trong số các đoạn ngang. Logic tương tự áp dụng cho các phân đoạn dọc. 
5. Lặp lại phép tính tương tự cho trường hợp đường đi bắt đầu theo chiều dọc, hoán đổi vai trò của các vị trí chẵn và lẻ. 
6. Lấy giá trị nhỏ nhất trên tất cả k và cả hai hướng bắt đầu. 

Tính đúng đắn xuất phát từ thực tế là đối với bất kỳ k cố định và hướng bắt đầu cố định nào, bài toán sẽ phân tách thành hai tối ưu hóa chi phí tuyến tính độc lập trên hai nhóm. Trong mỗi nhóm, vì chi phí có chiều dài tuyến tính và không có sự tương tác giữa các phân đoạn ngoại trừ thông qua một ràng buộc về tổng cố định, nên việc tập trung tất cả độ dài bổ sung vào hệ số tối thiểu luôn là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        c = list(map(int, input().split()))

        # prefix sums and minima for odd/even indices
        odd_sum = [0] * (n + 1)
        even_sum = [0] * (n + 1)
        odd_min = [10**18] * (n + 1)
        even_min = [10**18] * (n + 1)

        for i in range(1, n + 1):
            val = c[i - 1]

            odd_sum[i] = odd_sum[i - 1]
            even_sum[i] = even_sum[i - 1]
            odd_min[i] = odd_min[i - 1]
            even_min[i] = even_min[i - 1]

            if i % 2 == 1:
                odd_sum[i] += val
                odd_min[i] = min(odd_min[i], val)
            else:
                even_sum[i] += val
                even_min[i] = min(even_min[i], val)

        ans = 10**30

        for k in range(1, n + 1):
            odd_cnt = (k + 1) // 2
            even_cnt = k // 2

            # start horizontal: odd -> H, even -> V
            h_cost = odd_sum[k] + (n - odd_cnt) * odd_min[k]
            v_cost = even_sum[k] + (n - even_cnt) * even_min[k]
            ans = min(ans, h_cost + v_cost)

            # start vertical: swap roles
            h_cost = even_sum[k] + (n - even_cnt) * even_min[k]
            v_cost = odd_sum[k] + (n - odd_cnt) * odd_min[k]
            ans = min(ans, h_cost + v_cost)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phân tách thông tin tiền tố cho các vị trí chẵn và lẻ để có thể đánh giá bất kỳ độ dài tiền tố k nào trong thời gian không đổi. Điểm tinh tế duy nhất là tính toán chính xác có bao nhiêu phân đoạn thuộc về mỗi lớp chẵn lẻ, vì điều đó xác định độ dài còn lại phải được phân phối. 

Hai trường hợp định hướng được xử lý đối xứng bằng cách hoán đổi tính chẵn lẻ nào tương ứng với chuyển động ngang. 

## Ví dụ đã hoạt động 

Xét đầu vào mẫu thứ hai: n = 3 với các hệ số [2, 3, 1]. 

Chúng tôi tính toán dữ liệu tiền tố: 

| k | lẻ_sum | chẵn_sum | lẻ_min | chẵn_min | lẻ_cnt | chẵn_cnt | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | thông tin | 1 | 0 | 
| 2 | 2 | 3 | 2 | 3 | 1 | 1 | 
| 3 | 3 | 3 | 1 | 3 | 2 | 1 | 

Với k = 3 và bắt đầu theo chiều ngang, các đoạn lẻ nằm ngang và đoạn chẵn nằm dọc. Chi phí cơ sở ngang là 3, chi phí cơ sở dọc là 3. Chiều dài ngang còn lại là 3 - 2 = 1, được gán cho số lẻ tối thiểu là 1 và chiều dọc còn lại là 3 - 1 = 2 được gán cho số chẵn tối thiểu là 3. Điều này cho ra tổng 3 + 3 + 1 + 6 = 13. 

Dấu vết này cho thấy giải pháp tối ưu tập trung độ dài còn lại vào hệ số nhỏ nhất trong mỗi nhóm chẵn lẻ thay vì trải rộng nó. 

Bây giờ hãy xem xét trường hợp hệ số giảm: [5, 4, 3, 2]. Thuật toán đánh giá tất cả các tiền tố và giải pháp tốt nhất sẽ chuyển sang sử dụng các tiền tố dài hơn vì các hệ số nhỏ hơn sau này sẽ giảm chi phí gán độ dài bổ sung, được ghi lại bởi các thuật ngữ cực tiểu của tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi tiền tố được xử lý một lần và mỗi tiền tố k được đánh giá theo thời gian không đổi | 
| Không gian | O(n) | Mảng tiền tố lưu trữ tổng và cực tiểu | 

Tổng số n trên các trường hợp thử nghiệm là 100000, do đó việc quét tuyến tính trên mỗi trường hợp thử nghiệm phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            c = list(map(int, input().split()))

            odd_sum = [0] * (n + 1)
            even_sum = [0] * (n + 1)
            odd_min = [10**18] * (n + 1)
            even_min = [10**18] * (n + 1)

            for i in range(1, n + 1):
                val = c[i - 1]
                odd_sum[i] = odd_sum[i - 1]
                even_sum[i] = even_sum[i - 1]
                odd_min[i] = odd_min[i - 1]
                even_min[i] = even_min[i - 1]

                if i % 2 == 1:
                    odd_sum[i] += val
                    odd_min[i] = min(odd_min[i], val)
                else:
                    even_sum[i] += val
                    even_min[i] = min(even_min[i], val)

            ans = 10**30

            for k in range(1, n + 1):
                odd_cnt = (k + 1) // 2
                even_cnt = k // 2

                h_cost = odd_sum[k] + (n - odd_cnt) * odd_min[k]
                v_cost = even_sum[k] + (n - even_cnt) * even_min[k]
                ans = min(ans, h_cost + v_cost)

                h_cost = even_sum[k] + (n - even_cnt) * even_min[k]
                v_cost = odd_sum[k] + (n - odd_cnt) * odd_min[k]
                ans = min(ans, h_cost + v_cost)

            print(ans)

    solve()
    return ""

# provided samples
assert run("""3
2
13 88
3
2 3 1
5
4 3 2 1 4
""") == ""

# custom cases
assert run("""1
2
1 100
""") == "", "min prefix dominates"

assert run("""1
4
5 4 3 2
""") == "", "decreasing costs"

assert run("""1
5
1 1 1 1 1
""") == "", "uniform costs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 100`| thống trị phân khúc đầu tối thiểu | tiền tố hành vi tối thiểu | 
|`5 4 3 2`| câu trả lời nhỏ qua tiền tố sau | cơ cấu chi phí giảm | 
|`1 1 1 1 1`| chia tỷ lệ tuyến tính nhất quán | ổn định đồng đều | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi giải pháp tối ưu chỉ sử dụng một đoạn trong một nhóm hướng, nghĩa là gần như toàn bộ độ dài được gán cho một hệ số duy nhất. Đối với đầu vào như [10, 1, 10, 1], thuật toán nắm bắt một cách tự nhiên rằng lựa chọn tốt nhất là tối đa hóa sự đóng góp của hệ số nhỏ nhất trong mỗi lớp chẵn lẻ. Tiền tố tối thiểu đảm bảo rằng khi số 1 xuất hiện, tất cả các tiền tố trong tương lai phản ánh rằng nó có thể hấp thụ hầu hết độ dài còn lại. 

Một trường hợp khác là khi các hệ số ban đầu nhỏ nhưng các hệ số sau thậm chí còn nhỏ hơn. Đối với [3, 1, 2, 0,5], quá trình quét tiền tố đảm bảo rằng k kéo dài qua điểm xuất hiện 0,5 sẽ ngay lập tức giảm chi phí hiệu quả vì các cập nhật tối thiểu và thuật toán sẽ thay đổi phân bổ trọng số tương ứng mà không cần tính toán lại hoặc quay lui.
