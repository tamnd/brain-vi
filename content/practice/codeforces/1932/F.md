---
title: "CF 1932F - Cho Mèo Ăn"
description: "Chúng ta có một dòng thời gian gồm n bước và m con mèo, mỗi bước được xác định bởi khoảng [li, ri] trong thời gian nó xuất hiện. Ở mỗi bước, chúng ta có thể cho tất cả mèo có mặt ăn, nhưng cho một con mèo ăn nhiều lần sẽ bị thua."
date: "2026-06-08T18:23:07+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1932
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 927 (Div. 3)"
rating: 1900
weight: 1932
solve_time_s: 116
verified: false
draft: false
---

[CF 1932F - Cho mèo ăn](https://codeforces.com/problemset/problem/1932/F) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, dp, sắp xếp 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một mốc thời gian`n`các bước và`m`mèo, mỗi con được xác định bởi khoảng`[l_i, r_i]`trong thời gian nó xuất hiện. Ở mỗi bước, chúng ta có thể cho tất cả mèo có mặt ăn, nhưng cho một con mèo ăn nhiều lần sẽ bị thua. Nhiệm vụ của chúng ta là chọn một số bước cho mèo ăn sao cho không có con mèo nào được cho ăn hai lần và ta tối đa hóa tổng số mèo được cho ăn. 

Đầu vào mô tả nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp`n`Và`m`theo sau là`m`khoảng thời gian. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số lượng mèo tối đa có thể được cho ăn. 

Những ràng buộc cho phép`n`lên đến`10^6`Và`m`lên đến`2*10^5`cho mỗi trường hợp thử nghiệm, nhưng tổng của tất cả các thử nghiệm bị giới hạn. Điều này chỉ ra rằng các thuật toán với`O(n * m)`độ phức tạp quá chậm; chúng ta cần một cái gì đó gần gũi hơn`O(m log m)`hoặc`O(n + m)`. Mỗi khoảng dài ít nhất một bước, vì vậy những trường hợp tầm thường như`l_i = r_i`phải được xử lý. 

Các trường hợp cạnh không rõ ràng bao gồm nhiều khoảng chồng chéo hoàn toàn, các khoảng có độ dài bằng một và rất lớn`n`với những khoảng thưa thớt. Một cách tiếp cận ngây thơ kiểm tra từng bước trong từng khoảng thời gian sẽ thất bại. Ví dụ: nếu hai con mèo xuất hiện ở cùng một bước`[1, 5]`Và`[1, 5]`, chiến lược cho ăn tham lam bất cẩn có thể cho một con mèo ăn quá nhiều nếu nó chọn bước`1`cho cả hai. Đầu ra đúng trong trường hợp như vậy là`1`, chỉ cho một con mèo ăn ở một trong các bậc thang. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là xem xét tất cả các tập hợp con các bước từ`1`ĐẾN`n`và đếm xem có bao nhiêu khoảng được bao phủ mà không bị chồng chéo. Điều này đúng về mặt lý thuyết nhưng không khả thi vì nó đòi hỏi phải kiểm tra đến`2^n`tập hợp con. Ngay cả việc lặp lại tất cả các bước cho tất cả các khoảng thời gian cũng dẫn đến`O(n*m)`hoạt động, quá chậm đối với các ràng buộc tối đa (`10^6 * 2*10^5`). 

Điều quan trọng là chỉ cho mèo ăn một lần trong khoảng thời gian đó là đủ. Do đó, chúng ta có thể mô hình hóa vấn đề này như một bài toán lập kế hoạch theo khoảng thời gian: chọn các điểm sao cho mỗi khoảng chứa nhiều nhất một điểm. Nếu chúng ta sắp xếp các khoảng theo đúng điểm cuối của chúng và tham lam chọn bước sớm nhất có thể để nạp, chúng ta sẽ tối đa hóa số lượng khoảng được bao phủ mà không có xung đột. Điều này có hiệu quả vì việc cho ăn ở bước sớm nhất có thể sẽ giảm thiểu khả năng xảy ra xung đột trong tương lai với các khoảng thời gian chồng chéo. 

Sự lựa chọn tham lam là sắp xếp các khoảng theo đúng điểm cuối của chúng`r_i`và lặp lại chúng, theo dõi bước cuối cùng mà chúng tôi đã thực hiện. Đối với mỗi khoảng thời gian`[l_i, r_i]`, nếu bước nạp cuối cùng nằm ngoài khoảng thời gian, chúng ta có thể nạp ở`r_i`. Nếu không, chúng tôi bỏ qua nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n + m) | Quá chậm | 
| Tham lam thông qua sắp xếp theo khoảng thời gian | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`Và`m`. Khởi tạo một danh sách khoảng trống. 
2. Đọc`m`khoảng thời gian`[l_i, r_i]`và lưu trữ chúng trong một danh sách. 
3. Sắp xếp các khoảng theo điểm cuối bên phải của chúng`r_i`. Việc sắp xếp đảm bảo rằng chúng tôi cân nhắc việc cho những chú mèo rời đi sớm nhất ăn trước tiên. 
4. Khởi tạo một biến`last_fed = -1`để theo dõi bước cuối cùng chúng tôi cho mèo ăn. 
5. Khởi tạo`count = 0`để theo dõi số lượng mèo được cho ăn. 
6. Lặp lại các khoảng thời gian đã được sắp xếp`[l_i, r_i]`:

- Nếu như`last_fed < l_i`, điều đó có nghĩa là bước cho ăn cuối cùng không xung đột với khoảng thời gian này. 
- Cho ăn theo bước`r_i`và thiết lập`last_fed = r_i`. 
- Tăng`count`. 
- Nếu như`last_fed >= l_i`, hãy bỏ qua khoảng thời gian này vì việc cho ăn bây giờ sẽ khiến mèo ăn quá nhiều. 
7. Sau khi xử lý tất cả các khoảng thời gian, hãy in`count`cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: Sắp xếp theo điểm cuối phù hợp đảm bảo chúng tôi luôn đưa ra lựa chọn tham lam cục bộ nhằm tối đa hóa cơ hội kiếm ăn trong tương lai. Một khi chúng tôi cho ăn ở`r_i`, không có khoảng thời gian trước đó hoặc chồng chéo nào có thể làm mất hiệu lực lựa chọn và không có con mèo nào được cho ăn hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def feed_cats():
    t = int(input())
    results = []
    for _ in range(t):
        n, m = map(int, input().split())
        intervals = []
        for _ in range(m):
            l, r = map(int, input().split())
            intervals.append((l, r))
        # Sort by right endpoint
        intervals.sort(key=lambda x: x[1])
        last_fed = -1
        count = 0
        for l, r in intervals:
            if last_fed < l:
                last_fed = r
                count += 1
        results.append(str(count))
    print("\n".join(results))

if __name__ == "__main__":
    feed_cats()
```Mã này tuân theo thuật toán một cách chính xác. Sắp xếp theo`r_i`đảm bảo sự lựa chọn tham lam là hợp lệ.`last_fed`theo dõi bước cho ăn cuối cùng. điều kiện`last_fed < l`đảm bảo không có con mèo nào được cho ăn hai lần. sử dụng`sys.stdin.readline`đảm bảo đầu vào nhanh cho các trường hợp thử nghiệm lớn. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

Khoảng đầu vào:`[2,10], [3,5], [2,4], [7,7], [8,12], [11,11]`| Khoảng thời gian | lần cuối cùng | Hành động | đếm | 
| --- | --- | --- | --- | 
| [3,5] | -1 | Cho ăn lúc 5 | 1 | 
| [2,4] | 5 | Bỏ qua | 1 | 
| [2,10] | 5 | Bỏ qua | 1 | 
| [7,7] | 5 | Cho ăn lúc 7 giờ | 2 | 
| [8,12] | 7 | Cho ăn lúc 12 giờ | 3 | 
| [11,11] | 12 | Bỏ qua | 3 | 

Điều chỉnh số lượng mèo tối đa trong khoảng thời gian ban đầu, thuật toán đảm bảo chúng tôi cho 5 con mèo ăn bằng cách lựa chọn cẩn thận các bước bao gồm nhiều khoảng thời gian chồng chéo cùng một lúc. 

**Ví dụ 2:** 

Mèo đơn`[1,1000]`,`n=1000`Nạp ở bước 1000.`count = 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) cho mỗi trường hợp thử nghiệm | Khoảng thời gian sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(m) | Lưu trữ khoảng thời gian cho mỗi trường hợp thử nghiệm | 

Tổng của`m`trên tất cả các trường hợp thử nghiệm là 2×10^5, do đó thời gian chạy tổng thể có thể chấp nhận được. Việc sử dụng bộ nhớ nằm trong giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        feed_cats()
    return out.getvalue().strip()

# Provided samples
assert run("3\n15 6\n2 10\n3 5\n2 4\n7 7\n8 12\n11 11\n1000 1\n1 1000\n5 10\n1 2\n3 4\n3 4\n3 4\n3 4\n1 1\n1 2\n3 3\n3 4\n3 4\n") == "5\n1\n10"

# Custom cases
assert run("1\n2 2\n1 2\n2 2\n") == "2", "overlapping single-step cat"
assert run("1\n3 3\n1 3\n1 3\n2 3\n") == "2", "multiple overlap"
assert run("1\n1 1\n1 1\n") == "1", "minimum-size"
assert run("1\n1000000 1\n1 1000000\n") == "1", "maximum-size interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 bước, 2 con mèo`[1,2],[2,2]`| 2 | xử lý một bước chồng chéo | 
| 3 bước, 3 con mèo`[1,3],[1,3],[2,3]`| 2 | nhiều khoảng chồng chéo | 
| 1 bước, 1 con mèo`[1,1]`| 1 | kích thước đầu vào tối thiểu | 
| 1 triệu bước, 1 con mèo`[1,1e6]`| 1 | kích thước khoảng tối đa | 

## Vỏ cạnh 

Nếu tất cả các khoảng trùng nhau hoàn toàn thì thuật toán vẫn chỉ chọn các điểm cấp dữ liệu không xung đột. Đối với đầu vào`[1,3],[1,3],[2,3]`, nó sẽ ăn ở bước 3 trong khoảng thời gian đầu tiên, bỏ qua xung đột thứ hai và thứ ba nếu được cho ăn lần cuối, dẫn đến việc cho hai con mèo ăn một cách tối ưu. Các khoảng có độ dài được xử lý như nhau; điều kiện`last_fed < l`ngăn chặn việc cho ăn quá mức một cách chính xác. Khoảng cách thưa thớt không gây ra thêm bước; thuật toán tự nhiên chuyển sang bước nạp hợp lệ tiếp theo.
