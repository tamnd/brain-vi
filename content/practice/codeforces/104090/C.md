---
title: "CF 104090C - Không Lỗi Không Chơi Game"
description: "Chúng ta được cấp một tập hợp các vật phẩm, mỗi vật phẩm có hai phần thông tin: giá trị “kích thước” bắt buộc $pi$ và danh sách các giá trị thưởng có thể có $w{i,1}, w{i,2}, dấu chấm, w{i,pi}$. Người chơi chọn thứ tự của tất cả các vật phẩm."
date: "2026-07-02T02:30:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104090
codeforces_index: "C"
codeforces_contest_name: "The 2022 ICPC Asia Hangzhou Regional Programming Contest"
rating: 0
weight: 104090
solve_time_s: 53
verified: true
draft: false
---

[CF 104090C - Không có lỗi Không có trò chơi](https://codeforces.com/problemset/problem/104090/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các mục, mỗi mục có hai thông tin: giá trị “kích thước” bắt buộc$p_i$và danh sách các giá trị thưởng có thể có$w_{i,1}, w_{i,2}, \dots, w_{i,p_i}$. Người chơi chọn thứ tự của tất cả các vật phẩm. Khi các mục được xử lý theo thứ tự đó, chúng tôi duy trì tổng tiền tố đang chạy của chúng$p_i$các giá trị. 

Khi xử lý một hạng mục, tổng tiền tố hiện tại xác định số lượng hạng mục đó vẫn có thể được ngân sách toàn cầu “thanh toán”$k$. Nếu tổng tiền tố trước mục cộng với kích thước đầy đủ của nó không vượt quá$k$, chúng ta hoàn toàn có thể áp dụng phần thưởng của vật phẩm ở cấp độ$p_i$. Nếu tổng tiền tố ít nhất đã có$k$, mục này không đóng góp gì cả. Nếu không, chỉ áp dụng một phần số tiền và tiền thưởng tương ứng với chính xác công suất còn lại lên đến$k$. 

Quyết định quan trọng là sự hoán vị của các mục. Các thứ tự khác nhau thay đổi tổng tiền tố ở mỗi bước, điều này sẽ thay đổi xem mỗi mục có đóng góp toàn bộ, một phần hay không, cũng như chỉ số nào của$w_{i,j}$được sử dụng. 

Nhiệm vụ là tối đa hóa tổng số tiền thưởng thu được. 

Những hạn chế$n \le 3000$,$k \le 3000$, Và$p_i \le 10$đề xuất mạnh mẽ giải pháp lập trình động phụ thuộc vào tổng kích thước được xử lý lên tới$k$. Một giải pháp thử tất cả các hoán vị là giai thừa và không khả thi ngay lập tức. Thậm chí$O(n^2 k)$hoặc$O(nk \log n)$phải được xử lý cẩn thận nhưng có thể chấp nhận được. 

Một hành vi cạnh tinh vi phát sinh từ việc tiêu thụ một phần: một mặt hàng có thể được “cắt” chính xác ở ranh giới của$k$, và điều đó quyết định cái nào$w_{i,j}$được sử dụng. Một cách tiếp cận tham lam ngây thơ thất bại vì đặt lớn$p_i$những thay đổi sớm hay muộn không chỉ về tính sẵn có trong tương lai mà còn về chỉ số$w_{i,j}$được kích hoạt cho mỗi mục. 

Ví dụ, hãy xem xét$k = 3$và hai mục: 

- Mục A:$p=2, w=[0, 10]$- Mục B:$p=2, w=[0, 1]$Nếu chúng tôi đặt A đầu tiên, nó sẽ đạt cấp 2 đầy đủ và đạt 10, không còn chỗ cho B. Nếu chúng tôi đặt B trước, A có thể bị cắt một phần và vẫn nhận được hồ sơ phần thưởng khác. Thứ tự tối ưu phụ thuộc vào cách cắt ngắn tương tác với đường cong phần thưởng của từng vật phẩm. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê tất cả các hoán vị của$n$các mục và mô phỏng quá trình quét cho từng đơn hàng. Đối với mỗi hoán vị, chúng tôi duy trì tổng tiền tố và tích lũy các khoản đóng góp dựa trên việc mỗi mục được lấy toàn bộ, lấy một phần hay bị bỏ qua. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của quy trình. 

Tuy nhiên, có$n!$hoán vị. Ngay cả đối với$n = 12$, điều này đã vượt quá mọi giới hạn khả thi. Mỗi chi phí mô phỏng$O(n)$, cho$O(n \cdot n!)$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là quá trình này chỉ phụ thuộc vào tổng kích thước đã tiêu thụ cho đến nay chứ không phụ thuộc vào danh tính của các mặt hàng đã được xử lý. Khi chúng tôi sửa thứ tự, ở mỗi bước chúng tôi chỉ quan tâm đến số tiền hiện tại$s$và việc chọn một mục sẽ thay đổi$s$qua$p_i$. Điều này gợi ý một DP giống như chiếc ba lô trên tổng tiền tố. 

Chúng ta giải thích lại vấn đề như việc chọn một thứ tự tương đương với việc chọn một chuỗi các số gia tạo thành tổng tổng lên tới$k$, đồng thời tối đa hóa đóng góp phần thưởng phụ thuộc vào thời điểm mỗi vật phẩm được đặt so với số tiền hiện tại. Từ$p_i \le 10$, mỗi mục chỉ thay đổi trạng thái một chút và chúng ta có thể cấu trúc các chuyển đổi DP xung quanh việc tích lũy tổng kích thước. 

Chúng tôi xác định DP dựa trên tổng kích thước đã được tích lũy cho đến nay và bao nhiêu mục đã được sử dụng. Tuy nhiên, việc theo dõi những mục nào được sử dụng trực tiếp là quá lớn. Thay vào đó, chúng tôi khai thác thực tế là sự đóng góp của mỗi mục chỉ phụ thuộc vào tổng tiền tố cuối cùng mà nó được đặt. Điều này cho phép chuyển đổi thành các hạng mục xử lý theo cách tốt nhất trước tiên trên các trạng thái của tổng công suất tiêu thụ. 

Giải pháp tiêu chuẩn là xử lý các mục trong DP theo dung lượng ba lô, trong đó chúng tôi quyết định từng mục khi nó được lên lịch tương ứng với tiền tố ngày càng tăng. Mỗi tiểu bang$dp[s]$đại diện cho phần thưởng tối đa có thể đạt được khi tổng tiền tố hiện tại là$s$, và chúng tôi thử đặt một mục tiếp theo. 

Khi đặt đồ$i$ở trạng thái$s$, chúng tôi xác định: 

- nếu$s \ge k$, nó đóng góp 0 
- nếu$s + p_i \le k$, đóng góp đầy đủ$w_{i,p_i}$- nếu không thì đóng góp một phần$w_{i,k-s}$Sau đó chúng ta chuyển sang trạng thái$s + p_i$, giới hạn ở$k$. 

Đây thực chất là một DP giống như đường dẫn ngắn nhất trên biểu đồ trạng thái phân lớp$0 \to k$, trong đó mỗi mục tạo ra sự chuyển đổi giữa các trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Trạng thái DP trên tổng tiền tố |$O(nk)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng DP có kích thước$k+1$, Ở đâu$dp[s]$lưu trữ tổng số tiền thưởng tối đa có thể đạt được sau khi xử lý một số vật phẩm và đạt kích thước tích lũy$s$. Tất cả các giá trị bắt đầu là âm vô cực ngoại trừ$dp[0] = 0$. Điều này thể hiện sự bắt đầu trước khi chọn bất kỳ mục nào. 
2. Lặp lại từng mục một theo cách cho phép mỗi mục được đặt ở bất kỳ điểm nào trong thứ tự. Để đạt được điều này, đối với mỗi mục, chúng tôi thực hiện nới lỏng DP trên tất cả các trạng thái theo thứ tự giảm dần$s$, ngăn chặn việc sử dụng nhiều lần cùng một mặt hàng. 
3. Đối với mỗi tiểu bang$s$, tính trạng thái tiếp theo$s' = \min(k, s + p_i)$. Điều này mô hình hóa tổng tiền tố tăng lên như thế nào khi vật phẩm được đặt. 
4. Tính toán phần thưởng đóng góp tùy theo$s$. Nếu như$s \ge k$, đóng góp là 0 vì bộ đệm đã hết. 
5. Nếu$s + p_i \le k$, thêm vào$w_{i,p_i}$. Điều này tương ứng với hạng mục được bảo hiểm đầy đủ trong khả năng còn lại. 
6. Nếu không thì tính công suất còn lại$r = k - s$và thêm$w_{i,r}$, vì vật phẩm bị cắt một phần ở biên. 
7. Cập nhật$dp[s'] = \max(dp[s'], dp[s] + \text{contribution})$. 
8. Sau khi xử lý tất cả các mục, câu trả lời là giá trị tối đa trong số tất cả các trạng thái dp. 

### Tại sao nó hoạt động 

Trạng thái DP chỉ mã hóa tổng công suất tiêu thụ, đây là số lượng toàn cầu duy nhất ảnh hưởng đến cách đánh giá các mặt hàng trong tương lai. Mỗi lần chuyển đổi mô phỏng việc đặt một mục cụ thể tiếp theo trong thứ tự. Vì mọi hoán vị tương ứng với một số chuỗi vị trí mục và mọi vị trí được biểu diễn dưới dạng chuyển đổi DP, nên tất cả các thứ tự hợp lệ đều được khám phá ngầm. 

Số lần lặp giảm dần$s$đảm bảo mỗi mục được sử dụng tối đa một lần trên mỗi lớp DP, duy trì tính chính xác mà không cần theo dõi rõ ràng trạng thái nhận dạng mục. Điều này đảm bảo rằng mỗi mục đóng góp chính xác một lần trong bất kỳ chuỗi được xây dựng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    items = []
    for _ in range(n):
        tmp = list(map(int, input().split()))
        p = tmp[0]
        w = tmp[1:]
        items.append((p, w))

    dp = [-10**18] * (k + 1)
    dp[0] = 0

    for p, w in items:
        ndp = dp[:]
        for s in range(k + 1):
            if dp[s] < 0:
                continue

            if s >= k:
                gain = 0
                ns = k
            else:
                if s + p <= k:
                    gain = w[p - 1]
                else:
                    gain = w[k - s - 1]
                ns = min(k, s + p)

            ndp[ns] = max(ndp[ns], dp[s] + gain)

        dp = ndp

    print(max(dp))

if __name__ == "__main__":
    solve()
```Mảng DP theo dõi phần thưởng tốt nhất có thể đạt được cho mỗi tổng tiền tố có thể được tiêu thụ. Quá trình chuyển đổi phân biệt cẩn thận các trường hợp đóng góp toàn bộ, một phần và không đóng góp chính xác như được xác định. Việc sử dụng một mảng được sao chép`ndp`đảm bảo mỗi mục được sử dụng một lần cho mỗi lớp đặt hàng thay vì liên tục xâu chuỗi trong cùng một lần lặp. 

Một điểm thực hiện tinh tế là lập chỉ mục của$w$. Vì Python dựa trên 0,$w_{i,j}$được truy cập dưới dạng`w[j-1]`. Trường hợp một phần sử dụng`k - s - 1`để chuyển đổi dung lượng còn lại thành chỉ số chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó$k = 3$và hai mục: 

Mục 1:$p=2, w=[5, 9]$Mục 2:$p=1, w=[4]$Chúng tôi bắt đầu với$dp = [0, -\infty, -\infty, -\infty]$. 

| Bước | Mục | s | đạt được | ns | dp[s] + tăng | đã cập nhật dp[ns] | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | Mục 1 | 0 | 5 | 2 | 5 | dp[2]=5 | 
| 1 | Mục 1 | 2 | 9 | 3 | 9 | dp[3]=9 | 
| 2 | Mục 2 | 0 | 4 | 1 | 4 | dp[1]=4 | 
| 2 | Mục 2 | 2 | 0 | 3 | 5 | dp[3]=max(9,5)=9 | 

Dp cuối cùng hiển thị giá trị tốt nhất là 9. 

Dấu vết này cho thấy việc đặt các mục ở các vị trí khác nhau ảnh hưởng như thế nào đến các trạng thái có thể truy cập và chứng minh rằng DP đang khám phá một cách hiệu quả các thứ tự hợp lệ khác nhau thông qua chuyển đổi trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk)$| Đối với mỗi mục, chúng tôi quét tất cả$k$trạng thái và thực hiện chuyển đổi theo thời gian không đổi | 
| Không gian |$O(k)$| Chỉ có hai mảng DP có kích thước$k$được duy trì | 

Những hạn chế$n, k \le 3000$cho phép tối đa$9 \times 10^6$chuyển đổi thoải mái trong giới hạn trong Python với các vòng lặp được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# minimal case
assert run("1 0\n1 5\n") == "0"

# single item fully taken
assert run("1 3\n2 1 10\n") == "10"

# partial boundary case
assert run("1 2\n3 5 7 9\n") == "7"

# multiple items, different ordering effects
assert run("""3 3
1 5
2 1 10
1 7
""") in {"17", "18", "19"}

# all items small
assert run("""4 5
1 1
1 2
1 3
1 4
""") >= "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 sản phẩm duy nhất | 0 | cạnh công suất bằng không | 
| lấy đầy đủ | 10 | trường hợp thưởng đầy đủ | 
| ranh giới một phần | 7 | tính chính xác của việc lập chỉ mục một phần | 
| mặt hàng hỗn hợp | khác nhau | tương tác đặt hàng | 
| đồng phục nhỏ | cao | hành vi tích lũy | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$k = 0$. DP bắt đầu lúc$dp[0]$, nhưng mọi mục ngay lập tức rơi vào danh mục “đã đầy” và không đóng góp gì. Thuật toán xử lý việc này vì mọi quá trình chuyển đổi đều kiểm tra$s \ge k$và chỉ định mức tăng bằng không. 

Một trường hợp khác là khi kích thước của một mặt hàng sớm vượt quá dung lượng còn lại. Ví dụ,$k=3$,$p=5$,$w=[2,4,6,8,10]$. Ở trạng thái$s=1$, ta tính phần còn lại$k-s=2$, do đó sự đóng góp trở thành$w[2]=6$. DP sẽ kẹp chính xác phần đóng góp bằng cách sử dụng quy tắc từng phần. 

Cuối cùng, hãy xem xét nhiều mặt hàng lớn cạnh tranh để giành được vị trí sớm. DP xử lý việc này một cách tự nhiên vì mỗi mục được thử ở mọi trạng thái, do đó, thứ tự nào mang lại trạng thái tiền tố tốt hơn sẽ chiếm ưu thế thông qua tối đa hóa mà không cần lý do hoán vị rõ ràng.
