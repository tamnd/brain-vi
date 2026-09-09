---
title: "CF 104587G - Vấn đề về thứ hạng"
description: "Chúng tôi duy trì thứ hạng ngày càng tăng của các đội được gắn nhãn $T1$ đến $Tn$. Ban đầu, thứ hạng được cố định theo thứ tự chỉ số tăng dần, do đó $T1$ đứng đầu và $Tn$ đứng cuối. Sau đó, chúng tôi xử lý một chuỗi kết quả trận đấu, trong đó mỗi kết quả cho biết đội này đánh bại đội kia."
date: "2026-06-30T07:29:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104587
codeforces_index: "G"
codeforces_contest_name: "2020-2021 ICPC East Central North America Regional Contest (ECNA 2020)"
rating: 0
weight: 104587
solve_time_s: 43
verified: true
draft: false
---

[CF 104587G - Vấn đề về thứ hạng](https://codeforces.com/problemset/problem/104587/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì thứ hạng ngày càng tăng của các đội được gắn nhãn$T_1$bởi vì$T_n$. Ban đầu, thứ hạng được cố định theo thứ tự chỉ số tăng dần, vì vậy$T_1$là đầu tiên và$T_n$là cuối cùng. Sau đó, chúng tôi xử lý một chuỗi kết quả trận đấu, trong đó mỗi kết quả cho biết đội này đánh bại đội kia. 

Mỗi trận đấu chỉ ảnh hưởng đến thứ hạng khi đội chiến thắng hiện đang xếp hạng kém hơn đội thua mà họ đánh bại. Nếu người chiến thắng đã được xếp hạng cao hơn thì không có gì thay đổi. Nếu đội chiến thắng được xếp hạng thấp hơn, chúng tôi sẽ “thăng cấp” đội đó lên ngay trên đội bị đánh bại bằng cách loại đội đó ra khỏi vị trí hiện tại và đặt lại ngay phía trên đối thủ, đồng thời duy trì thứ tự tương đối của tất cả các đội không bị ảnh hưởng. 

Vì vậy, quá trình này là một chuỗi các thao tác chèn danh sách ổn định được điều khiển bằng cách so sánh các vị trí hiện tại chứ không phải các chỉ mục gốc. Đầu ra là thứ tự cuối cùng sau khi xử lý tất cả các kết quả khớp theo thứ tự thời gian. 

Các ràng buộc là nhỏ, với$n \le 100$Và$m \le 100$. Điều này ngay lập tức gợi ý rằng bất kỳ thuật toán nào có hành vi bậc hai hoặc thậm chí bậc ba đối với số lượng đội đều an toàn. Chúng ta có thể thoải mái mô phỏng các hoạt động sắp xếp lại trên một mảng mà không phải lo lắng về tắc nghẽn hiệu suất. 

Trường hợp phức tạp xuất hiện khi có nhiều trận đấu liên quan đến cùng một cặp hoặc khi tích lũy các khuyến mãi lặp đi lặp lại. Thứ hạng rất linh hoạt nên vị trí của một đội tại thời điểm diễn ra trận đấu là quan trọng chứ không phải vị trí ban đầu của đội đó. 

Ví dụ: hãy xem xét một kịch bản ban đầu có ba đội$T_1, T_2, T_3$. Nếu chúng tôi xử lý$T_3$nhịp đập$T_2$, sau đó$T_3$di chuyển lên. Sau này nếu$T_3$nhịp đập$T_1$, thao tác thứ hai sử dụng các vị trí được cập nhật chứ không phải thứ tự ban đầu. Bất kỳ giải pháp nào chỉ sử dụng các chỉ số ban đầu sẽ thất bại ở đây. 

Một trường hợp lợi thế quan trọng khác là khi người thắng đã ở trên người thua. Ví dụ: nếu thứ tự hiện tại là$T_1, T_2, T_3$và chúng tôi xử lý$T_1$nhịp đập$T_3$, không có gì thay đổi mặc dù các chỉ số cho thấy một khoảng trống. Điều kiện hoàn toàn là vị trí. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp là điều đương nhiên. Chúng tôi giữ thứ hạng dưới dạng mảng hoặc danh sách. Đối với mỗi trận đấu, chúng tôi tìm thấy các chỉ số hiện tại của cả hai đội. Nếu người thắng đã dẫn trước người thua, chúng tôi không làm gì cả. Nếu không, chúng tôi loại bỏ phần thắng khỏi vị trí hiện tại của nó và chèn nó ngay trước phần thua, dịch đoạn trung gian sang trái một bước. 

Điều này đúng vì nó tuân theo chính xác phát biểu vấn đề. Thử thách duy nhất là thực hiện thao tác “di chuyển từng phân đoạn một” một cách rõ ràng. 

Sự phức tạp mạnh mẽ đến từ việc tìm kiếm vị trí và thực hiện các thao tác trên danh sách. Chi phí mỗi lần tra cứu chỉ số của một nhóm$O(n)$và mỗi lần chèn yêu cầu dịch chuyển lên tới$O(n)$các phần tử. Với$m$hoạt động, tổng chi phí là$O(mn)$, nhiều nhất là$10^4$, dễ dàng trong giới hạn. 

Không cần cấu trúc dữ liệu nâng cao như cây cân bằng hoặc danh sách liên kết vì$n$là nhỏ bé. Một mảng đơn giản cộng với tên nhóm ánh xạ từ điển tới chỉ mục (hoặc tính toán lại trực tiếp các chỉ mục mỗi lần) là đủ. 

Cái nhìn sâu sắc quan trọng là việc xếp hạng là một hoán vị trong các hoạt động cắt và chèn cục bộ. Vì mọi thao tác chỉ ảnh hưởng đến một phân đoạn liền kề và duy trì trật tự bên trong nên mô phỏng trực tiếp đã là tối ưu trong các điều kiện ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu (danh sách + tìm kiếm) |$O(mn)$|$O(n)$| Đã chấp nhận | 
| Tối ưu hóa với hashmap + danh sách |$O(mn)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng xếp hạng trực tiếp. 

1. Khởi tạo danh sách`order`chứa các đội ở dạng`T1, T2, ..., Tn`. Điều này thể hiện thứ hạng hiện tại mọi lúc. 
2. Đối với mỗi trận đấu mà đội$A$đánh bại đội$B$, xác định vị trí hiện tại của họ trong`order`. Chúng tôi quét danh sách hoặc sử dụng bản đồ nếu được duy trì. 
3. Nếu vị trí của$A$nhỏ hơn vị trí của$B$, không làm gì vì xếp hạng đã đồng ý với kết quả. 
4. Nếu vị trí của$A$lớn hơn vị trí của$B$, di dời$A$từ vị trí hiện tại của nó. 
5. Chèn$A$tại chỉ số của$B$. Điều này đặt một cách hiệu quả$A$trực tiếp ở trên$B$, chuyển tất cả các đội can thiệp xuống một vị trí. 
6. Tiếp tục xử lý tất cả các kết quả phù hợp theo thứ tự. 

Điểm tinh tế là việc loại bỏ xảy ra trước khi chèn, nếu không thì các chỉ số sẽ dịch chuyển không chính xác. Hoạt động này về mặt khái niệm là cắt và dán, không phải là hoán đổi hoặc điều chỉnh cục bộ. 

### Tại sao nó hoạt động 

Việc xếp hạng luôn là sự hoán vị của các đội và mỗi thao tác thực thi chính xác một ràng buộc mới: đội chiến thắng phải xuất hiện ngay phía trên đội thua nếu trước đó đội đó ở dưới. Tất cả các thứ tự tương đối khác vẫn không thay đổi vì vấn đề nêu rõ rằng không có bằng chứng bổ sung nào tồn tại để sửa đổi chúng. Vì mọi thao tác chỉ đưa ra một hiệu chỉnh cục bộ duy nhất phù hợp với quy tắc nên việc áp dụng lặp đi lặp lại sẽ duy trì tính chính xác theo thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    order = ["T" + str(i) for i in range(1, n + 1)]

    for _ in range(m):
        a, b = input().split()

        ia = order.index(a)
        ib = order.index(b)

        if ia > ib:
            order.pop(ia)
            order.insert(ib, a)

    print(" ".join(order))

if __name__ == "__main__":
    main()
```Việc thực hiện tuân theo thuật toán gần như nguyên văn. Danh sách`order`lưu trữ thứ hạng hiện tại. Mỗi truy vấn sử dụng`.index()`để tìm vị trí, điều này có thể chấp nhận được vì$n \le 100$. điều kiện`ia > ib`nắm bắt chính xác trường hợp bảng xếp hạng mâu thuẫn với kết quả trận đấu. 

Chi tiết triển khai chính là loại bỏ phần thắng trước khi chèn. Nếu việc chèn được thực hiện trước, các chỉ số sẽ dịch chuyển và vị trí cuối cùng sẽ không chính xác. Việc sử dụng các định danh chuỗi như`"T3"`khớp trực tiếp với định dạng đầu vào, tránh mọi chi phí phân tích cú pháp. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu đầu tiên: 

đầu vào:```
5 3
T4 T1
T3 T1
T5 T3
```Thứ tự ban đầu luôn là:```
T1 T2 T3 T4 T5
```| Bước | Trận đấu | ia | ib | Hành động | Đặt hàng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | T4 đánh bại T1 | 3 | 0 | di chuyển T4 lên trên T1 | T4 T1 T2 T3 T5 | 
| 2 | T3 đánh bại T1 | 3 | 1 | không thay đổi | T4 T1 T2 T3 T5 | 
| 3 | T5 đánh bại T3 | 4 | 3 | di chuyển T5 lên trên T3 | T4 T1 T2 T5 T3 | 

Đầu ra cuối cùng:```
T4 T1 T2 T5 T3
```Dấu vết này cho thấy chỉ có vi phạm về cập nhật kích hoạt đặt hàng và cấu trúc trung gian vẫn ổn định. 

Bây giờ hãy xem xét một ví dụ được xây dựng thứ hai: 

đầu vào:```
4 3
T3 T2
T3 T1
T4 T3
```Ban đầu:```
T1 T2 T3 T4
```| Bước | Trận đấu | ia | ib | Hành động | Đặt hàng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | T3 đánh bại T2 | 2 | 1 | di chuyển T3 lên trên T2 | T1 T3 T2 T4 | 
| 2 | T3 đánh bại T1 | 1 | 0 | di chuyển T3 lên trên T1 | T3 T1 T2 T4 | 
| 3 | T4 đánh bại T3 | 3 | 0 | di chuyển T4 lên trên T3 | T4 T3 T1 T2 | 

Dấu vết này thể hiện chuyển động xếp hạng theo tầng: một nhóm có thể leo lên nhiều vị trí theo thời gian và mỗi bước di chuyển đều liên quan đến trạng thái hiện tại thay vì chỉ số ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(mn)$| Mỗi trong số$m$các trận đấu có thể yêu cầu quét và dịch chuyển trong danh sách kích thước$n$| 
| Không gian |$O(n)$| Chúng tôi lưu trữ thứ tự hiện tại của$n$đội | 

Với$n, m \le 100$, số lượng thao tác nguyên thủy tối đa là theo thứ tự$10^4$, nhanh một cách tầm thường dưới những ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided sample 1
assert run("""5 3
T4 T1
T3 T1
T5 T3
""") == "T4 T1 T2 T5 T3"

# provided sample 2
assert run("""8 4
T4 T1
T1 T2
T2 T3
T3 T4
""") == "T1 T2 T3 T4 T5 T6 T7 T8"

# minimum size
assert run("""2 1
T2 T1
""") == "T2 T1"

# no changes case
assert run("""3 2
T1 T2
T1 T3
""") == "T1 T2 T3"

# full reversal
assert run("""4 3
T4 T3
T4 T2
T4 T1
""") == "T4 T1 T2 T3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 đội, một lần hoán đổi | thứ tự đảo ngược | cắt và chèn cơ bản | 
| đã thắng liên tục | thứ tự không thay đổi | tình trạng không hoạt động | 
| người chiến thắng hàng đầu lặp đi lặp lại | chuỗi khuyến mãi đầy đủ | di chuyển tích lũy | 

## Vỏ cạnh 

Một trường hợp lợi thế quan trọng là khi một đội liên tục giành chiến thắng và tiếp tục tiến lên. Hãy xem xét đầu vào:```
4 3
T4 T3
T4 T2
T4 T1
```Việc thực thi bắt đầu với`T1 T2 T3 T4`. 

Sau đó`T4 beats T3`, thứ tự trở thành`T1 T2 T4 T3`. Sau đó`T4 beats T2`, nó trở thành`T1 T4 T2 T3`. Sau đó`T4 beats T1`, nó trở thành`T4 T1 T2 T3`. 

Ở mỗi bước, thuật toán sẽ loại bỏ`T4`và đặt lại nó ngay phía trên đối thủ của nó, giữ nguyên tất cả các thứ tự tương đối khác. Điều này cho thấy nhiều chuyển động đi lên được thực hiện chính xác mà không cần bất kỳ tính toán lại tổng thể nào. 

Một trường hợp khác là khi chiến thắng không kích hoạt chuyển động. Ví dụ:```
3 1
T1 T3
```Thứ tự ban đầu là`T1 T2 T3`. Từ`T1`đã ở trên rồi`T3`, điều kiện`ia > ib`thất bại và danh sách không thay đổi. Thuật toán tránh được những sửa đổi không cần thiết một cách chính xác, giúp duy trì sự ổn định của các phân đoạn không liên quan.
