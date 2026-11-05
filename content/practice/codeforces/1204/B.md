---
title: "CF 1204B - Mislove Đã Mất Một Mảng"
description: "Chúng ta được cho một mảng có độ dài n gồm các số nguyên dương, nhưng mảng này không phải là mảng tùy ý. Mọi giá trị trong đó phải đáp ứng quy tắc cấu trúc nghiêm ngặt: mỗi số là 1 hoặc là số chẵn có một nửa cũng xuất hiện ở đâu đó trong mảng."
date: "2026-06-13T15:41:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1204
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 581 (Div. 2)"
rating: 900
weight: 1204
solve_time_s: 173
verified: true
draft: false
---

[CF 1204B - Mislove Đã Mất Một Mảng](https://codeforces.com/problemset/problem/1204/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chiều dài`n`mảng các số nguyên dương, nhưng mảng không phải là mảng tùy ý. Mọi giá trị trong đó phải thỏa mãn một quy tắc cấu trúc chặt chẽ: mỗi số là một trong hai`1`, hoặc đó là một số chẵn mà một nửa của nó cũng nằm ở đâu đó trong mảng. Điều này có nghĩa là mọi giá trị có thể được truy tìm bằng cách chia liên tục cho hai cho đến khi chúng ta đạt được`1`và tất cả các giá trị trung gian trên chuỗi đó cũng phải tồn tại trong mảng. 

Ngoài hạn chế về cấu trúc đó, chúng ta còn được biết điều gì đó về sự đa dạng. Mảng phải chứa giữa`l`Và`r`những giá trị riêng biệt, bao hàm. Chúng tôi không được yêu cầu xây dựng một mảng như vậy, chỉ để xác định tổng nhỏ nhất và lớn nhất có thể có của các phần tử của nó trên tất cả các mảng hợp lệ. 

Các ràng buộc đủ nhỏ để`n`nhiều nhất là 1000 và số lượng giá trị riêng biệt nhiều nhất là 20. Điều đó ngay lập tức gợi ý rằng chúng ta không khám phá các không gian giá trị tùy ý; cấu trúc của các số hợp lệ là cực kỳ hạn chế. Mỗi số hợp lệ thuộc về một chuỗi nhân đôi bắt đầu từ`1`, vì vậy các giá trị trông giống như lũy thừa của hai, nhưng được phép lặp lại. 

Một điểm tinh tế là việc “đóng cửa dưới sự chia cho hai” buộc bất kỳ tập hợp giá trị riêng biệt hợp lệ nào trở thành tiền tố của chuỗi nhân đôi bắt đầu từ`1`. Nếu chúng ta bao gồm`8`, sau đó`4`,`2`, Và`1`cũng phải có mặt. Điều này giúp loại bỏ mọi sự kết hợp bất thường như`{1, 2, 8}`không có`4`. 

Trường hợp cạnh xuất hiện khi`l`Và`r`hạn chế số lượng các giá trị riêng biệt một cách chặt chẽ. Ví dụ, nếu`l = r = 1`, mảng duy nhất có thể là tất cả các mảng, vì bất kỳ giá trị nào lớn hơn đều đưa ra các phần tử bắt buộc bổ sung. Một góc khác là khi`n`lớn nhưng`r`nhỏ, buộc phải lặp lại nhiều số nhỏ. 

Một cách tiếp cận ngây thơ cố gắng liệt kê tất cả các tập hợp con giá trị hợp lệ và gán bội số sẽ nhanh chóng trở thành tổ hợp, vì số lượng tập hợp con có kích thước lên tới 20 vẫn còn lớn và việc gán số lượng sẽ thêm một lớp hàm mũ khác. Cấu trúc quyền lực của hai người là điều tạo nên khả năng xây dựng tham lam. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng chọn một tập hợp các giá trị riêng biệt thỏa mãn quy tắc đóng, sau đó phân phối`n`vị trí giữa chúng và tính tổng kết quả. Đối với mỗi tập ứng cử viên, chúng ta sẽ đảm bảo rằng nếu một số`x`được bao gồm, tất cả các giá trị`x/2, x/4, ..., 1`cũng được bao gồm. Ngay cả khi chúng tôi giới hạn bản thân ở các chuỗi hợp lệ, chúng tôi vẫn cần xem xét mỗi giá trị xuất hiện bao nhiêu lần. 

Số lượng chuỗi hợp lệ có thể có là nhỏ vì mọi tập hợp riêng biệt hợp lệ đều được xác định đầy đủ bởi phần tử tối đa của nó, nhưng phân phối bội số vẫn linh hoạt. Việc liệt kê các phân phối một cách thô bạo dẫn đến sự phức tạp theo cấp số nhân trong`n`, đại khái là theo thứ tự phân vùng của`n`trên tối đa 20 giá trị, điều này là không khả thi. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến các tiền tố của dãy`1, 2, 4, 8, ...`. Nếu chúng ta cố định số lượng giá trị riêng biệt`k`, thì tập hợp các giá trị tối ưu bị ép buộc: nó phải là`{1, 2, 4, ..., 2^(k-1)}`. Không có quyền tự do lựa chọn cấu trúc vượt quá độ dài của tiền tố này. 

Khi chúng tôi sửa tập hợp các giá trị, tổng được xác định bằng số lần chúng tôi gán mỗi giá trị. Vì các giá trị lớn hơn về tổng thể thì “đắt” hơn, nên tổng tối đa đạt được bằng cách gán càng nhiều phần tử càng tốt cho các giá trị lớn nhất được phép trong khi tôn trọng rằng chúng ta vẫn phải giữ cho chuỗi hợp lệ. Tổng tối thiểu thì ngược lại: chúng tôi ưu tiên các giá trị nhỏ hơn càng nhiều càng tốt, nhưng chúng tôi vẫn phải đảm bảo tồn tại đủ các giá trị khác biệt. 

Điều này làm giảm vấn đề để cố gắng tất cả những gì có thể`k`TRONG`[l, r]`, và với mỗi`k`, tính toán số tiền tối thiểu và tối đa tốt nhất có thể theo chuỗi bắt buộc`{1, 2, ..., 2^(k-1)}`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong n và tập hợp con | Hàm mũ | Quá chậm | 
| Tối ưu | O(r) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta tính toán trước chuỗi lũy thừa của 2 vì mọi giá trị hợp lệ đều xuất phát từ chuỗi này. 

1. Với mỗi số giá trị riêng biệt có thể có`k`từ`l`ĐẾN`r`, chúng tôi xây dựng tập hợp các giá trị chuẩn`v_i = 2^(i-1)`vì`i`từ`1`ĐẾN`k`. Điều này bị ép buộc bởi ràng buộc về khả năng chia hết, vì bất kỳ cấu trúc hợp lệ nào cũng phải là chuỗi tiền tố. 
2. Đối với cố định`k`, chúng tôi xác định cách phân phối`n`sự xuất hiện trong số này`k`giá trị trong khi vẫn duy trì hiệu lực. Ràng buộc không hạn chế số lượng ngoài việc cho phép lặp lại, vì vậy mọi phân phối đều hợp lệ miễn là tất cả`k`các giá trị xuất hiện ít nhất một lần nếu được yêu cầu. 
3. Để giảm thiểu tổng, trước tiên chúng ta gán càng nhiều phần tử càng tốt cho các giá trị nhỏ nhất. Đây là một quá trình lấp đầy tham lam: chúng tôi đặt một lần xuất hiện của mỗi giá trị bắt buộc để thỏa mãn tính khác biệt, sau đó gán các vị trí còn lại cho giá trị nhỏ nhất`1`. 
4. Để tối đa hóa tổng, chúng ta làm ngược lại. Chúng tôi chỉ định một lần xuất hiện của mỗi giá trị bắt buộc, sau đó phân bổ các vị trí còn lại cho giá trị lớn nhất`2^(k-1)`. 
5. Chúng tôi tính tổng kết quả cho mỗi`k`, theo dõi mức tối thiểu và tối đa toàn cầu trên tất cả các giá trị hợp lệ`k`. 

Chi tiết quan trọng là sau khi cố định độ dài tiền tố, vấn đề trở nên tuyến tính vì cấu trúc chi phí đơn điệu: lũy thừa lớn hơn của hai luôn lấn át lũy thừa nhỏ hơn, do đó phép gán tham lam là tối ưu theo cả hai hướng. 

### Tại sao nó hoạt động 

Thuộc tính đóng buộc tất cả các bộ giá trị riêng biệt hợp lệ tạo thành tiền tố của chuỗi nhân đôi. Khi độ dài tiền tố này được cố định, không có lựa chọn cấu trúc bổ sung nào, chỉ có phép gán bội số. Vì tất cả các đóng góp vào tổng đều là tuyến tính và được sắp xếp theo độ lớn, nên tính tối ưu xuất phát từ việc phân bổ số lượng tham lam đến giá trị nhỏ nhất hoặc lớn nhất tùy thuộc vào việc chúng ta giảm thiểu hay tối đa hóa. Không có sự sắp xếp lại nào có thể cải thiện kết quả vì việc hoán đổi bất kỳ đơn vị khối lượng nào giữa giá trị nhỏ hơn và giá trị lớn hơn sẽ làm thay đổi tổng theo một hướng cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, l, r = map(int, input().split())

    # precompute powers of two up to 20 (safe upper bound)
    pw = [1]
    for _ in range(1, 25):
        pw.append(pw[-1] * 2)

    min_ans = float('inf')
    max_ans = 0

    for k in range(l, r + 1):
        # construct values 1,2,4,...,2^(k-1)
        vals = pw[:k]

        # MINIMUM: put one of each, rest go to 1
        min_sum = sum(vals)
        min_sum += (n - k) * vals[0]

        # MAXIMUM: put one of each, rest go to largest
        max_sum = sum(vals)
        max_sum += (n - k) * vals[-1]

        min_ans = min(min_ans, min_sum)
        max_ans = max(max_ans, max_sum)

    print(min_ans, max_ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng lũy ​​thừa của hai để chúng ta có thể lập chỉ mục trực tiếp các giá trị được yêu cầu. Đối với mỗi số lượng riêng biệt có thể`k`, nó giả sử cấu trúc hợp lệ duy nhất là cấu trúc đầu tiên`k`sức mạnh của hai. Sau đó, nó tính tổng cơ sở trong đó mỗi giá trị xuất hiện một lần. Từ đó, nó phân phối phần còn lại`n - k`các phần tử thành giá trị nhỏ nhất hoặc lớn nhất tùy thuộc vào việc chúng ta đang giảm thiểu hay tối đa hóa. 

Một lỗi phổ biến là quên rằng phải tồn tại ít nhất một lần xuất hiện của mỗi giá trị riêng biệt, đó là lý do tại sao tổng cơ sở bao gồm chính xác một trong mỗi giá trị trước khi phân phối các giá trị bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2 2
```Chúng tôi chỉ xem xét`k = 2`, do đó các giá trị là`[1, 2]`. 

| Bước | Giá trị | Tổng cơ sở | Còn lại | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| k=2 | [1,2] | 3 | 2 | gán cho 1 | 3 + 2 = 5 | 
| k=2 | [1,2] | 3 | 2 | gán cho 2 | 3 + 4 = 7 | 

Tối thiểu là 5, tối đa là 7. 

Điều này cho thấy rằng một khi cấu trúc đã được cố định thì chỉ có sự phân bổ các phần tử còn lại mới quan trọng. 

### Ví dụ 2 

đầu vào:```
5 1 3
```Chúng tôi kiểm tra`k = 1, 2, 3`. 

Vì`k = 1`, giá trị`[1]`: tổng =`5`. 

Vì`k = 2`, giá trị`[1,2]`: căn cứ`3`, còn lại`3`. 

| k | căn cứ | phút | tối đa | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | 5 | 
| 2 | 3 | 5 | 7 | 
| 3 | 7 | 7 + 2*1 = 9 | 7 + 2*4 = 15 | 

Chúng tôi lấy tối thiểu toàn cầu`5`và tối đa`15`. 

Điều này cho thấy việc tăng các giá trị khác biệt sẽ buộc các phần tử bắt buộc phải lớn hơn như thế nào do ràng buộc nhân đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(r) | Chúng tôi thử từng số giá trị riêng biệt có thể lên tới 20 | 
| Không gian | O(1) | Chỉ một mảng lũy ​​thừa cố định của hai được lưu trữ | 

Những ràng buộc đảm bảo`r ≤ 20`, vì vậy vòng lặp này là thời gian không đổi trong thực tế. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isfinite
    # re-implement solution inline for testing
    n, l, r = map(int, input().split())

    pw = [1]
    for _ in range(1, 25):
        pw.append(pw[-1] * 2)

    min_ans = float('inf')
    max_ans = 0

    for k in range(l, r + 1):
        vals = pw[:k]
        base = sum(vals)
        min_ans = min(min_ans, base + (n - k) * vals[0])
        max_ans = max(max_ans, base + (n - k) * vals[-1])

    return f"{min_ans} {max_ans}"

# provided sample
assert run("4 2 2") == "5 7"

# all ones forced
assert run("1 1 1") == "1 1"

# small chain
assert run("3 1 2") == "3 5"

# larger spread
assert run("5 1 3") == "5 15"

# tight distinct bound
assert run("10 3 3") == str(sum([1,2,4]) + 7) + " " + str(sum([1,2,4]) + 7*4)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 1 | trường hợp cạnh một giá trị | 
| 4 2 2 | 5 7 | số lượng khác biệt cố định | 
| 3 1 2 | 3 5 | phân nhánh nhỏ | 
| 10 3 3 | bắt nguồn | tiền tố bắt buộc có kích thước 3 | 

## Vỏ cạnh 

Khi nào`l = r = 1`, cấu trúc sụp đổ chỉ còn giá trị`1`, vì bất kỳ giá trị nào lớn hơn sẽ buộc phải có thêm các phần tử riêng biệt. Thuật toán xử lý việc này vì`k = 1`mang lại một danh sách giá trị duy nhất`[1]`, và cả tối thiểu và tối đa đều trở thành`n`. 

Khi`k`lớn, chẳng hạn như`k = 20`, các giá trị tăng theo cấp số nhân, do đó, ngay cả một sự bao gồm duy nhất của một lũy thừa lớn cũng chiếm ưu thế trong tổng. Phép gán tham lam đảm bảo rằng các phần tử bổ sung luôn đi đến một trong hai`1`hoặc`2^(k-1)`, nắm bắt chính xác hành vi cực đoan mà không cần bất kỳ lý luận tổ hợp nào. 

Nếu như`n`bằng`k`, không có phần tử bổ sung nào để phân phối và câu trả lời chỉ đơn giản là tổng của phần tử đầu tiên`k`sức mạnh của hai. Việc triển khai xử lý việc này vì`(n - k)`trở thành số 0, chỉ còn lại tổng cơ số.
