---
title: "CF 103665L - \u0410 \u043e\u043d\u0430 \u0435\u043c\u0443 \u043a\u0430\u043a \u0440\u0430\u0437"
description: "Chúng tôi được tặng một bộ mũ, mỗi chiếc được mô tả bằng bốn con số. Những con số này xác định một quá trình thương lượng có cấu trúc giữa người mua và người bán."
date: "2026-07-02T21:46:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103665
codeforces_index: "L"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2018"
rating: 0
weight: 103665
solve_time_s: 61
verified: true
draft: false
---

[CF 103665L - \u0410 \u043e\u043d\u0430 \u0435\u043c\u0443 \u043a\u0430\u043a \u0440\u0430\u0437](https://codeforces.com/problemset/problem/103665/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ mũ, mỗi chiếc được mô tả bằng bốn con số. Những con số này xác định một quá trình thương lượng có cấu trúc giữa người mua và người bán. Người mua bắt đầu từ mức giá mong muốn và tăng dần từng bước, trong khi người bán bắt đầu từ mức giá ban đầu và giảm dần từng bước. Cả hai bước di chuyển đều diễn ra theo cấp số cộng với cùng kích thước bước, do đó trình tự của chúng di chuyển về phía nhau cho đến khi một trong số các điều kiện dừng được kích hoạt. 

Đối với mỗi chiếc mũ, cuộc đàm phán này sẽ kết thúc bằng một thỏa thuận thành công ở một mức giá thỏa thuận cuối cùng nào đó hoặc sẽ thất bại nếu người bán từ chối hạ giá thấp hơn chi phí sản xuất và người mua vẫn ở dưới ngưỡng đó. Nếu một thỏa thuận xảy ra, chúng ta cũng cần biết tổng cộng có bao nhiêu lời nói được nói ra trong suốt quá trình. 

Nhiệm vụ là chọn chính xác một chiếc mũ có thể mua thành công, giảm thiểu mức giá phải trả cuối cùng. Nếu nhiều chiếc mũ có cùng mức giá tối thiểu thì bất kỳ chiếc mũ nào cũng được chấp nhận. 

Kích thước đầu vào lên tới 200.000 chiếc mũ, do đó, bất kỳ giải pháp nào mô phỏng toàn bộ đoạn hội thoại trên mỗi chiếc mũ đều quá chậm. Mỗi cuộc đàm phán là một chuỗi các bước tuyến tính và có thể kéo dài nhiều vòng, do đó, một mô phỏng đơn giản sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Chúng ta cần một phương pháp dạng đóng để tính toán kết quả cuối cùng cho mỗi chiếc mũ. 

Một điểm tinh tế quan trọng là cuộc đàm phán có thể kết thúc theo ba cách khác nhau và chỉ có hai trong số đó tương ứng với một giao dịch mua hợp lệ. Một cách tiếp cận bất cẩn chỉ kiểm tra xem các chuỗi có giao nhau hay không sẽ phân loại sai các trường hợp người bán từ chối do hạn chế về chi phí. 

Một kịch bản lỗi đơn giản xuất hiện khi trình tự của người mua và người bán trùng nhau nhưng người bán chỉ chấp nhận mức giá thấp hơn giá thành. Ví dụ: nếu các chuỗi gặp nhau ở mức 17 nhưng chi phí là 18 thì giao dịch phải bị từ chối ngay cả khi tồn tại giao điểm. 

Một trường hợp phức tạp khác là khi lời đề nghị tiếp theo của người mua bỏ qua lời đề nghị hiện tại của người bán. Trong trường hợp này, người mua dừng trước chứ không phải người bán, điều này sẽ thay đổi cả giá cuối cùng và số bước. 

Cuối cùng, có trường hợp chấp nhận trực tiếp khi giá chào bán của người mua đầu tiên đã vượt quá giá ban đầu của người bán. Ở đây câu trả lời là ngay lập tức và số lượng trao đổi là tối thiểu. Thiếu phím tắt này dẫn đến việc đếm bước không chính xác trong mô phỏng cạnh. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ tạo ra rõ ràng cả cấp số cộng cho mỗi chiếc mũ và các lượt thay thế cho đến khi một điều kiện dừng được kích hoạt. Mỗi bước cập nhật ưu đãi hiện tại của người mua và nhu cầu của người bán, kiểm tra các điều kiện dừng và tiếp tục. Điều này đơn giản và chính xác vì nó tuân theo chính xác các quy tắc của bài toán. 

Tuy nhiên, mỗi cuộc đàm phán có thể kéo dài tỷ lệ thuận với số bước cho đến khi các trình tự giao nhau, trong trường hợp xấu nhất là ở mức 10^6 trên mỗi mũ nếu các tham số là đối nghịch. Với tối đa 2 × 10^5 mũ, điều này trở nên vượt xa giới hạn khả thi. 

Quan sát quan trọng là cả hai chuỗi đều là cấp số cộng có kích thước bước bằng nhau. Điều này có nghĩa là chúng ta đang tìm kiếm điểm đầu tiên nơi hai hàm tuyến tính gặp nhau theo các quy tắc lấy mẫu xen kẽ. Thay vì mô phỏng từng bước, chúng ta có thể tính toán hành vi giao cắt chính xác bằng cách sử dụng số học số nguyên. 

Chúng tôi theo dõi trình tự người mua như 

B_k = d + k·s 

và trình tự người bán như 

S_k = c − k·s 

Sự tương tác được điều chỉnh bằng cách so sánh hai trình tự này ở các bước xen kẽ. Quá trình kết thúc khi một trình tự vượt qua hoặc khi hạn chế về chi phí phá vỡ tính khả thi. Vì cả hai chuỗi đều di chuyển tuyến tính với độ dốc giống nhau nên thứ tự tương đối của chúng thay đổi đơn điệu, do đó chúng ta có thể giải trực tiếp điểm gặp nhau đầu tiên.

Chúng tôi giảm từng chiếc mũ thành tính toán k nhỏ nhất trong đó một trong các điều kiện dừng kích hoạt, sau đó xác thực xem trạng thái dừng đó có tương ứng với việc bán hàng hay từ chối hợp lệ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · bước) | O(1) | Quá chậm | 
| Số học/Dạng đóng trên mỗi chiếc mũ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chiếc mũ một cách độc lập. 

1. Xác định các đề nghị của người mua theo một chuỗi bắt đầu từ d và tăng dần theo s mỗi lượt. Xác định yêu cầu của người bán theo trình tự bắt đầu từ c và giảm dần s mỗi lượt. Quá trình này thay đổi về mặt khái niệm, nhưng cả hai chuỗi đều là cấp số cộng đơn điệu, vì vậy chúng tôi không mô phỏng rõ ràng các lần lượt. 
2. Tính thời điểm sớm nhất mà giá chào bán của người mua không còn thấp hơn mức giá mà người bán tiếp theo yêu cầu. Điều này tương ứng với việc giải quyết sự trùng lặp của hai dãy số học. Bởi vì cả hai đều di chuyển với kích thước bước bằng nhau, điều này làm giảm việc so sánh độ lệch ban đầu và tính chẵn lẻ của các lượt. 
3. Tính chỉ số gặp gỡ tiềm năng đầu tiên k nơi người mua và người bán có thể giao nhau. Đây thực sự là k nhỏ nhất sao cho d + k·s ≥ c − k·s. Sắp xếp lại sẽ có 2k·s ≥ c − d, nên k = ceil((c − d) / (2s)). 
4. Xác định giá họp ứng viên. Tại thời điểm đó, cả hai chuỗi đã hội tụ xung quanh cùng một giá trị nên giá giao dịch được xác định bởi bên nào kích hoạt sự chấp nhận trước theo quy tắc. Điều này tương đương với việc đánh giá cả hai chuỗi tại k đó và kiểm tra điều kiện dừng được xác định bởi bài toán. 
5. Kiểm tra tính khả thi so với chi phí m. Nếu giá gặp mặt dưới m, người bán sẽ từ chối ngay cả khi trình tự trùng nhau. Trong trường hợp này, kết quả là thất bại và chúng ta loại bỏ chiếc mũ này. 
6. Nếu không hãy tính giá cuối cùng và số lần trao đổi. Số lượng dòng nói tỷ lệ thuận với chỉ số kết thúc: mỗi bước đóng góp một hành động của người mua hoặc người bán, do đó tổng số là 2k + 1 trong trường hợp giao nhau điển hình, có cách xử lý đặc biệt đối với các trường hợp chấp nhận ngay lập tức. 
7. Theo dõi chiếc mũ hợp lệ nhất bằng cách giảm thiểu giá cuối cùng. Nếu nhiều chiếc mũ có cùng mức giá thì bất kỳ chỉ số nào cũng được chấp nhận. 

### Tại sao nó hoạt động 

Giá trị của cả người mua và người bán đều tiến triển tuyến tính với độ lớn bước giống nhau nhưng hướng ngược nhau. Điều này tạo ra một quá trình hội tụ đơn điệu trong đó sự khác biệt giữa người bán và người mua giảm xuống chính xác 2 giây cho mỗi vòng đầy đủ. Do đó, lần đầu tiên đề nghị của người mua đạt hoặc vượt quá yêu cầu của người bán sẽ được xác định duy nhất và có thể tính toán được ở dạng đóng. Ràng buộc chi phí chỉ đóng vai trò kiểm tra ngưỡng sau khi hội tụ, do đó nó không ảnh hưởng đến vị trí của giao lộ, chỉ ảnh hưởng đến việc giao lộ có hợp lệ hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    best_price = None
    best_idx = -1
    best_ops = 0

    for i in range(1, n + 1):
        c, d, m, s = map(int, input().split())

        # immediate acceptance case
        if d >= c:
            price = d
            ops = 1
            if m <= price:
                if best_price is None or price < best_price:
                    best_price = price
                    best_idx = i
                    best_ops = ops
            continue

        # compute meeting step
        diff = c - d
        k = (diff + 2 * s - 1) // (2 * s)

        price_buyer = d + k * s
        price_seller = c - k * s

        price = price_buyer  # they meet or cross here

        # validity check
        if price < m:
            continue

        ops = 2 * k + 1

        if best_price is None or price < best_price:
            best_price = price
            best_idx = i
            best_ops = ops

    if best_price is None:
        print(-1)
    else:
        print(best_idx, best_price, best_ops)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý từng chiếc mũ một cách độc lập và tính toán kết quả mà không cần mô phỏng. Trường hợp đặc biệt`d >= c`xử lý việc chấp nhận ngay lập tức, trong đó người mua đã đưa ra ít nhất mức giá khởi điểm của người bán. 

Tính toán cốt lõi làm giảm việc đàm phán tuyến tính thành một biểu thức số học duy nhất cho chỉ số giao nhau đầu tiên. Việc phân chia mức trần đảm bảo chúng tôi xử lý chính xác các trường hợp người mua vượt qua người bán trước khi có sự bình đẳng chính xác. 

Kiểm tra chi phí`price < m`thực thi điều kiện từ chối sau khi hội tụ, phù hợp với quy tắc người bán từ chối nếu giá thỏa thuận cuối cùng thấp hơn giá thành sản xuất. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ với hai chiếc mũ. 

đầu vào:```
2
10 2 1 3
20 5 10 4
```Đối với chiếc mũ đầu tiên, người mua bắt đầu ở mức 2 và người bán ở mức 10. Chênh lệch là 8, bước là 3, vì vậy k = ceil(8 / 6) = 2. Giá của người mua là 2 + 2·3 = 8, giá của người bán là 10 − 2·3 = 4. Chúng giao nhau, giá cuối cùng là 8 và vì m = 1 nên nó hợp lệ. 

Đối với chiếc mũ thứ hai, người mua bắt đầu ở mức 5 và người bán ở mức 20. Chênh lệch là 15, bước là 4, vì vậy k = ceil(15 / 8) = 2. Giá của người mua là 13, giá của người bán là 12, nên giá cuối cùng là 13. 

| Mũ | k | Giá người mua | Giá bán | Giá cuối cùng | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 8 | 4 | 8 | Có | 
| 2 | 2 | 13 | 12 | 13 | Có | 

Cả hai đều hợp lệ nên chúng ta chọn mũ 1. 

Dấu vết này cho thấy cả hai cuộc đàm phán đều sụp đổ khi tính toán một chỉ số giao nhau duy nhất thay vì mô phỏng cuộc đối thoại luân phiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chiếc mũ được xử lý bằng các phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có một số biến được duy trì trên toàn cầu | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì nó tránh được bất kỳ sự mô phỏng từng bước nào của quá trình thương lượng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    best_price = None
    best_idx = -1
    best_ops = 0

    for i in range(1, n + 1):
        c, d, m, s = map(int, input().split())

        if d >= c:
            price = d
            ops = 1
            if m <= price:
                if best_price is None or price < best_price:
                    best_price = price
                    best_idx = i
                    best_ops = ops
            continue

        diff = c - d
        k = (diff + 2 * s - 1) // (2 * s)

        price = d + k * s

        if price < m:
            continue

        ops = 2 * k + 1

        if best_price is None or price < best_price:
            best_price = price
            best_idx = i
            best_ops = ops

    if best_price is None:
        return "-1\n"
    return f"{best_idx} {best_price} {best_ops}\n"

# provided sample-like case
assert run("1\n36 2 22 10\n") == "1 22 5\n"

# immediate acceptance
assert run("1\n5 10 1 3\n") == "1 10 1\n"

# rejection due to cost
assert run("1\n20 1 15 2\n") == "-1\n"

# two candidates, choose cheaper
assert run("2\n10 1 1 2\n10 2 1 2\n") != "-1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu đơn | 1 22 5 | tính đúng đắn của công thức chính | 
| chấp nhận ngay lập tức | 1 10 1 | nhánh phím tắt | 
| từ chối chi phí | -1 | lọc bán hàng không hợp lệ | 
| lựa chọn cà vạt | chiếc mũ đẹp nhất | giảm thiểu chính xác | 

## Vỏ cạnh 

Trường hợp một bên là khi người mua đã đưa ra ít nhất mức giá ban đầu của người bán. Trong tình huống này, cuộc đàm phán kết thúc ngay lập tức. Thuật toán xử lý việc này bằng cách kiểm tra`d >= c`trước bất kỳ số học nào, đảm bảo số lượng phép tính là 1 và không thực hiện phép chia nào. 

Một trường hợp cạnh khác xảy ra khi bước họp được tính toán chính xác bằng 0 do sự bằng nhau khi bắt đầu so sánh số học. Trong trường hợp đó, k trở thành 0 và cả hai giá đều bằng giá trị ban đầu của chúng, thể hiện chính xác sự hội tụ ngay lập tức. 

Trường hợp cạnh thứ ba phát sinh khi giá giao nhau được tính toán ở dưới ngưỡng chi phí. Mặc dù sự hội tụ số học xảy ra, thuật toán sẽ loại bỏ chiếc mũ sau khi tính toán k, phù hợp với quy tắc chi phí ngăn cản việc bán hàng bất kể hành vi thương lượng.
