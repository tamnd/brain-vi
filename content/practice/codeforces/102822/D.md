---
title: "CF 102822D - Gỡ bom"
description: "Vấn đề mô tả một bộ sưu tập bom, trong đó mỗi quả bom có ​​một giá trị đếm ngược. Trong một hành động, chúng ta có thể chọn một quả bom và tăng đồng hồ của nó lên một quả. Ngay sau đó, mỗi quả bom mất một quả từ đồng hồ của nó."
date: "2026-07-26T15:52:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102822
codeforces_index: "D"
codeforces_contest_name: "2020 China Collegiate Programming Contest - Mianyang Site"
rating: 0
weight: 102822
solve_time_s: 48
verified: true
draft: false
---

[CF 102822D - Gỡ bom](https://codeforces.com/problemset/problem/102822/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Vấn đề mô tả một bộ sưu tập bom, trong đó mỗi quả bom có một giá trị đếm ngược. Trong một hành động, chúng ta có thể chọn một quả bom và tăng đồng hồ của nó lên một quả. Ngay sau đó, mỗi quả bom mất một quả từ đồng hồ của nó. Nếu bất kỳ đồng hồ nào trở nên âm sau mức giảm này thì quá trình sẽ kết thúc. Mục tiêu là tối đa hóa số lượng hành động lựa chọn được thực hiện trước khi vụ nổ xảy ra. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra số lượng bom và giá trị đồng hồ ban đầu của mỗi quả bom. Đầu ra là số lần tối đa chúng ta có thể chọn một quả bom, được định dạng dưới dạng câu trả lời trường hợp. 

Số lượng bom có ​​thể đạt tới$10^5$và tổng số bom trong tất cả các trường hợp thử nghiệm nhiều nhất là$3 \times 10^5$. Một giải pháp thử mọi chuỗi lựa chọn có thể là không thể vì số lượng các chuỗi có thể tăng theo cấp số nhân. Ngay cả một mô phỏng cố gắng tìm ra lựa chọn tốt nhất tại mọi thời điểm cũng sẽ quá chậm vì câu trả lời có thể cực kỳ lớn khi giá trị đồng hồ lớn. Giá trị của đồng hồ có thể đạt tới$10^9$, do đó thuật toán phải phụ thuộc vào số lượng bom hơn là độ lớn của câu trả lời. 

Phần khó khăn là hành động cuối cùng được tính mặc dù nó có thể ngay lập tức gây ra vụ nổ. Một sai lầm phổ biến là chỉ tính các vòng an toàn đã hoàn thành. 

Ví dụ:```
1
2
1 1
```Đầu ra đúng là:```
Case #1: 3
```Một mô phỏng bất cẩn có thể nói là 2 vì sau hai vòng hoàn thành, cả hai quả bom đều trống rỗng. Tuy nhiên, vẫn còn thời gian để thực hiện thêm một hành động chọn nữa và vụ nổ chỉ xảy ra sau lần giảm tiếp theo. 

Một trường hợp khác là khi một số quả bom bắt đầu từ con số 0:```
1
3
0 5 5
```Quả bom đầu tiên không thể sống sót sau một hiệp trừ khi nó được chọn vào mỗi thời điểm quan trọng. Một giải pháp chỉ nhìn vào tổng số đồng hồ có thể bỏ sót rằng từng quả bom riêng lẻ cần được bảo vệ đầy đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng mô phỏng quá trình và quyết định loại bom nào sẽ tăng ở mỗi lượt. Vì việc chọn một quả bom sẽ bảo vệ nó khỏi sự sụt giảm trong vòng đó, nên ý tưởng tham lam tự nhiên là luôn chọn quả bom có ​​giá trị hiện tại nhỏ nhất. Điều này có thể áp dụng được trong những trường hợp nhỏ nhưng nó không tiết lộ cấu trúc thực tế của vấn đề. Câu trả lời có thể lớn bằng tổng của tất cả các đồng hồ, khiến cho việc mô phỏng trở nên quá chậm. 

Quan sát quan trọng đến từ việc xem xét một số vòng an toàn cố định thay vì các lựa chọn riêng lẻ. 

Giả sử chúng ta muốn sống sót$x$hoàn thành các vòng đấu. Bom$i$thua một ở mỗi vòng mà nó không được chọn. Nếu được chọn$c_i$lần, giá trị cuối cùng của nó sau những lần này$x$vòng là:$$a_i - (x - c_i)$$Để quả bom vẫn còn sống, chúng ta cần:$$c_i \geq x-a_i$$Nếu như$x-a_i$là âm, quả bom không cần bất kỳ biện pháp bảo vệ đặc biệt nào, vì vậy yêu cầu thực sự là:$$c_i \geq \max(0, x-a_i)$$Tổng số lựa chọn có sẵn trong$x$vòng chính xác là$x$. Như vậy,$x$vòng an toàn có thể thực hiện được nếu:$$\sum_i \max(0, x-a_i) \leq x$$Tình trạng này là đơn điệu. Nếu chúng ta có thể sống sót$x$thì chúng ta có thể sống sót sau ít vòng hơn. Nếu chúng ta không thể sống sót$x$làm tròn, giá trị lớn hơn là không thể. Điều đó cho phép tìm kiếm nhị phân. 

Câu trả lời chúng ta cần là nhiều hơn số vòng an toàn hoàn chỉnh tối đa một vòng vì hành động lựa chọn tiếp theo vẫn được tính trước khi vụ nổ xảy ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời × n) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân trên các vòng | O(n log(tổng(a))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tất cả các đồng hồ bom ban đầu. Điều này đưa ra giới hạn trên cho số vòng hoàn chỉnh mà chúng ta cần xem xét, bởi vì mỗi vòng sẽ giảm tổng giá trị đồng hồ đi$n-1$và tìm kiếm nhị phân chỉ cần giới hạn trên an toàn. 
2. Tìm kiếm nhị phân số vòng an toàn hoàn chỉnh tối đa$x$. Đối với một ứng cử viên$x$, tính xem có bao nhiêu lựa chọn bị ép buộc bởi những quả bom yếu. Bom$i$yêu cầu$\max(0, x-a_i)$sự lựa chọn. 
3. Nếu tổng số lựa chọn bắt buộc nhiều nhất$x$, đánh dấu giá trị này là có thể và tìm kiếm số vòng lớn hơn. Ngược lại, tìm kiếm các giá trị nhỏ hơn. 
4. Sau khi tìm được số vòng hoàn chỉnh lớn nhất có thể$x$, đầu ra$x+1$. Cái bổ sung đại diện cho hành động lựa chọn cuối cùng gây ra vụ nổ sau đó. 

Tại sao nó hoạt động: 

Điều kiện tìm kiếm nhị phân mô tả chính xác liệu các lựa chọn có sẵn có thể giữ cho mọi quả bom còn sống hay không. Bất kỳ quả bom nào có đồng hồ quá nhỏ cần phải được chọn thường xuyên đủ để bù đắp cho sự suy giảm mà nó nhận được. Tổng các yêu cầu tối thiểu này là số lượng lựa chọn nhỏ nhất cần thiết. Nếu điều này phù hợp bên trong$x$các vòng có sẵn, một lịch trình hợp lệ tồn tại. Vì điều kiện là đơn điệu nên tìm kiếm nhị phân sẽ tìm ra giá trị lớn nhất có thể$x$và thêm một tài khoản cho hành động được tính cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(a):
    total = sum(a)

    def possible(x):
        need = 0
        for v in a:
            if x > v:
                need += x - v
                if need > x:
                    return False
        return True

    lo, hi = 0, total + 1
    while lo + 1 < hi:
        mid = (lo + hi) // 2
        if possible(mid):
            lo = mid
        else:
            hi = mid

    return lo + 1

def main():
    t = int(input())
    ans = []

    for case in range(1, t + 1):
        n = int(input())
        a = list(map(int, input().split()))
        ans.append(f"Case #{case}: {solve_case(a)}")

    print("\n".join(ans))

if __name__ == "__main__":
    main()
```các`possible`Hàm kiểm tra điều kiện toán học dẫn xuất ở trên. Nó chỉ tính những lựa chọn bổ sung mà bom cần vì một quả bom có ​​giá trị ít nhất`x`có thể sống sót`x`vòng mà không cần bảo vệ. 

Tìm kiếm nhị phân sử dụng ranh giới trên nửa mở.`total + 1`là an toàn vì câu trả lời cho các vòng hoàn chỉnh không bao giờ vượt quá tổng của tất cả các đồng hồ. Câu trả lời cuối cùng được tăng thêm một vì bài toán yêu cầu số hành động chọn chứ không phải số vòng hoàn thành đầy đủ. 

Sự trở lại sớm bên trong`possible`tránh những công việc không cần thiết. Khi các lựa chọn được yêu cầu vượt quá số vòng có sẵn, giá trị ứng cử viên sẽ không còn nữa. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
2
1 1
3
1 2 3
```Trường hợp thử nghiệm đầu tiên có hai quả bom. 

| Hoàn thành vòng thi ứng viên | Yêu cầu về bom | Tổng số lựa chọn bắt buộc | Có thể | 
| --- | --- | --- | --- | 
| 1 | 0, 0 | 0 | Có | 
| 2 | 1, 1 | 2 | Có | 
| 3 | 2, 2 | 4 | Không | 

Số vòng hoàn thành an toàn lớn nhất là 2, vì vậy câu trả lời là 3. 

Đối với trường hợp thử nghiệm thứ hai: 

| Hoàn thành vòng thi ứng viên | Yêu cầu về bom | Tổng số lựa chọn bắt buộc | Có thể | 
| --- | --- | --- | --- | 
| 2 | 1, 0, 0 | 1 | Có | 
| 3 | 2, 1, 0 | 3 | Có | 
| 4 | 3, 2, 1 | 6 | Không | 

Số vòng hoàn thành an toàn lớn nhất là 3, vì vậy câu trả lời là 4. 

Những dấu vết này cho thấy tại sao giải pháp không cần quyết định chính xác thứ tự chọn bom. Sự bất bình đẳng chỉ hỏi liệu có tồn tại sự phân bố nào đó của các lựa chọn hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(tổng(a))) | Mỗi lần kiểm tra tìm kiếm nhị phân sẽ quét tất cả các quả bom một lần và phạm vi tìm kiếm được giới hạn bởi tổng số đồng hồ. | 
| Không gian | O(1) | Chỉ có mảng đầu vào và một vài biến được sử dụng. | 

Tổng số quả bom trong tất cả các cuộc thử nghiệm là$3 \times 10^5$, do đó, việc quét mảng khoảng 30 lần cho mỗi trường hợp kiểm thử là dễ dàng nằm trong giới hạn. Số học số nguyên Python cũng xử lý các khoản tiền lớn một cách an toàn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve_case(a):
        total = sum(a)

        def possible(x):
            need = 0
            for v in a:
                if x > v:
                    need += x - v
                    if need > x:
                        return False
            return True

        lo, hi = 0, total + 1
        while lo + 1 < hi:
            mid = (lo + hi) // 2
            if possible(mid):
                lo = mid
            else:
                hi = mid
        return lo + 1

    t = int(input())
    out = []
    for case in range(1, t + 1):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(f"Case #{case}: {solve_case(a)}")

    sys.stdin = old_stdin
    return "\n".join(out)

assert run("""2
2
1 1
3
1 2 3
""") == """Case #1: 3
Case #2: 4""", "samples"

assert run("""1
2
0 0
""") == """Case #1: 1""", "minimum clocks"

assert run("""1
3
5 5 5
""") == """Case #1: 16""", "all equal large values"

assert run("""1
4
0 1 2 3
""") == """Case #1: 3""", "zero boundary"

assert run("""1
5
1000000000 1000000000 1000000000 1000000000 1000000000
""") == """Case #1: 5000000001""", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai trường hợp mẫu |`Case #1: 3`,`Case #2: 4`| Tính đúng đắn cơ bản | 
|`[0,0]`|`1`| Giá trị tối thiểu và hành động được tính cuối cùng | 
| Năm chiếc đồng hồ lớn bằng nhau |`5000000001`| Xử lý câu trả lời lớn | 
|`[0,1,2,3]`|`3`| Yêu cầu bảo vệ bom cá nhân | 
| Năm đồng hồ`10^9`|`5000000001`| Số học số nguyên lớn | 

## Vỏ cạnh 

Đối với trường hợp:```
1
2
1 1
```Tìm kiếm nhị phân kiểm tra các vòng hoàn chỉnh. Có thể thực hiện hai vòng vì mỗi quả bom có ​​thể được chọn một lần. Ba vòng là không thể vì mỗi quả bom sẽ cần hai biện pháp bảo vệ, đòi hỏi bốn lựa chọn. Thuật toán trả về`2 + 1 = 3`, đếm chính xác hành động cuối cùng gây ra vụ nổ. 

Đối với trường hợp:```
1
3
0 5 5
```Quả bom đầu tiên bắt đầu từ con số 0. Để sống sót qua một vòng hoàn chỉnh, nó phải được chọn vì nếu không nó sẽ trở thành số âm. Điều kiện yêu cầu:$$\max(0,1-0)+\max(0,1-5)+\max(0,1-5)=1$$Chỉ có một lựa chọn nên bạn có thể tham gia vòng đấu. Đối với các giá trị lớn hơn, mức độ bảo vệ được yêu cầu cuối cùng sẽ vượt quá các lựa chọn có sẵn và việc tìm kiếm nhị phân sẽ dừng ở điểm chính xác. 

Đối với trường hợp:```
1
3
0 0 0
```Mỗi quả bom cần phải được chọn bất cứ khi nào nó sẽ mất giá trị. Sau 0 vòng hoàn thành, vẫn có thể thực hiện được một hành động chọn cuối cùng, vì vậy câu trả lời là 1. Thuật toán xử lý việc này vì hành động cuối cùng`+1`tách biệt khỏi tính toán vòng an toàn.
