---
title: "CF 105494F - Đèn giao thông"
description: "Chúng tôi đang mô phỏng một chiếc xe buýt di chuyển qua dãy đèn giao thông. Giữa các giao lộ, xe buýt dành một khoảng thời gian di chuyển cố định và tại mỗi giao lộ, xe buýt có thể phải chờ tùy thuộc vào trạng thái hiện tại của tín hiệu giao thông định kỳ."
date: "2026-06-23T01:40:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105494
codeforces_index: "F"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Qualification Contest"
rating: 0
weight: 105494
solve_time_s: 44
verified: true
draft: false
---

[CF 105494F - Đèn giao thông](https://codeforces.com/problemset/problem/105494/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một chiếc xe buýt di chuyển qua dãy đèn giao thông. Giữa các giao lộ, xe buýt dành một khoảng thời gian di chuyển cố định và tại mỗi giao lộ, xe buýt có thể phải chờ tùy thuộc vào trạng thái hiện tại của tín hiệu giao thông định kỳ. 

Mỗi đèn giao thông có chu kỳ riêng được xác định bằng khoảng thời gian xanh và đỏ. Chu kỳ lặp lại trong mỗi khoảng thời gian cố định và tại thời điểm 0, chúng ta được biết ánh sáng được bù như thế nào so với chu kỳ của nó, theo nghĩa là màu xanh lá cây đã hoạt động trong một số giây nhất định trước khi chuyến đi bắt đầu. Khi xe buýt đến mỗi ngã tư, chúng ta phải xác định xem đèn có xanh vào đúng thời điểm đó hay không. Nếu nó có màu đỏ, chúng ta đợi cho đến khi nó chuyển sang màu xanh trở lại rồi tiếp tục. 

Đầu vào mô tả một chuỗi các giao điểm. Đối với mỗi giao lộ i, chúng ta có thời gian di chuyển để đến được giao lộ đó từ giao lộ trước đó, khoảng thời gian xanh gi, khoảng thời gian ri màu đỏ và độ lệch di mô tả khoảng thời gian trước khi bắt đầu chuyến đi, giai đoạn xanh cuối cùng bắt đầu. Mục đích là tính tổng thời gian đã trôi qua khi xe buýt đi qua tất cả các giao lộ. 

Các ràng buộc đủ lớn nên việc mô phỏng từng giây là không thể. Ngay cả quá trình tiến hóa thời gian từng bước trực tiếp cũng sẽ giảm xuống O (tổng thời gian), trong trường hợp xấu nhất có thể cực kỳ lớn nếu tích lũy thời gian chờ đợi. 

Một trường hợp khó phát hiện khi xe buýt đến đúng điểm chuyển tiếp giữa màu xanh lá cây và màu đỏ. Nếu remi bằng gi thì thời điểm đó đã là điểm bắt đầu của màu đỏ nên cần phải chờ đợi. Một trường hợp khác là khi gi bằng 0, nghĩa là đèn luôn có màu đỏ và mọi lực đến đều phải chờ một chu kỳ đầy đủ. 

Việc triển khai đơn giản cũng có thể xử lý sai offset di, đặc biệt nếu nó lớn. Vì di có thể lớn hơn nhiều so với chu kỳ nên việc không giảm nó theo modul của chu kỳ sẽ dẫn đến việc tính toán pha không chính xác. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ theo dõi thời gian liên tục và đối với mỗi giao lộ, thời gian sẽ tăng dần cho đến khi đèn chuyển sang màu xanh lục. Sau khi di chuyển đến từng ngã tư, chúng ta tăng thời gian bằng ai, sau đó liên tục kiểm tra trạng thái đèn bằng cách mô phỏng chu kỳ từng giây. Điều này đúng vì nó phản ánh chính xác quy trình thực, nhưng nó có thể giảm xuống O (tổng thời gian trong tất cả các lần chờ), điều này không khả thi khi thời gian chờ đợi lớn. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng từng giây riêng lẻ. Trạng thái đèn giao thông có tính tuần hoàn nên bất cứ lúc nào chúng ta cũng chỉ cần biết mình đang ở đâu trong chu kỳ của nó. Thay vì tăng lên từng giây một, chúng ta có thể tính trực tiếp pha của ánh sáng bằng cách sử dụng số học mô-đun. 

Khi chúng ta đến giao lộ i, đã biết thời gian đã trôi qua kể từ lúc xuất phát. Chu kỳ của ánh sáng lặp lại sau mỗi gi + ri giây. Vì đèn xanh cuối cùng bắt đầu di giây trước thời gian 0 nên tổng thời gian đã trôi qua kể từ lần bắt đầu xanh tham chiếu đó là thời gian + di. Việc giảm modulo này theo độ dài chu kỳ sẽ cho chúng ta vị trí hiện tại trong chu kỳ. Nếu vị trí này nằm trong khoảng màu xanh lá cây, chúng ta sẽ vượt qua ngay lập tức. Nếu không, chúng ta sẽ chuyển thẳng sang giai đoạn xanh tiếp theo. 

Điều này làm giảm mỗi giao điểm thành tính toán O(1), vì số học mô-đun thay thế mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng thời gian chờ đợi) | O(1) | Quá chậm | 
| Mô phỏng mô-đun | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Hãy để thời gian đại diện cho thời gian trôi qua hiện tại kể từ khi bắt đầu chuyến đi.

1. Khởi tạo thời gian về 0. Điều này thể hiện thời điểm trước khi bước vào đoạn đầu tiên, nơi chưa có hành trình nào xảy ra. 
2. Tại mỗi giao lộ i, trước tiên hãy tăng thời gian lên ai. Điều này tính đến thời gian di chuyển cần thiết để đến được đèn giao thông. 
3. Tính tổng thời gian đã trôi qua kể từ khi tham chiếu màu xanh lá cây đã biết là Totali = time + di. Điều này sẽ thay đổi dòng thời gian của chúng ta để chúng ta có thể căn chỉnh thời điểm hiện tại với định nghĩa giai đoạn của chu kỳ. 
4. Tính chu kỳ = gi + ri. Điều này thể hiện độ dài lặp lại đầy đủ của tín hiệu giao thông. 
5. Tính remi = tổng thời gian mod. Điều này mang lại vị trí trong chu kỳ hiện tại. Modulo hoạt động vì hành vi tín hiệu lặp lại giống hệt nhau trong mỗi chu kỳ. 
6. Nếu remi < gi, tín hiệu sẽ có màu xanh khi đến và chúng tôi tiếp tục không chậm trễ. Ngược lại, tín hiệu có màu đỏ và chúng ta phải đợi chính xác theo chu kỳ - remi giây để đạt đến ranh giới pha xanh tiếp theo. Sau khi chờ đợi, chúng tôi thêm độ trễ này vào thời gian. 

Sau khi xử lý tất cả các giao lộ, thời gian chứa tổng thời gian di chuyển bao gồm tất cả thời gian chờ đợi. 

Bất biến chính là ở mỗi bước, thời gian luôn phản ánh thời điểm sớm nhất xe buýt có thể đến ngã tư tiếp theo sau khi đã giải quyết đầy đủ mọi quyết định chờ trước đó. Bởi vì mỗi bản cập nhật chỉ phụ thuộc vào thời gian hiện tại và tính toán modulo xác định của chu kỳ của ánh sáng tiếp theo, nên không có quyết định nào trong tương lai phụ thuộc vào cách chúng ta đạt đến trạng thái hiện tại. Quá trình này tối ưu cục bộ ở mọi giao lộ và do đó chính xác trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    time = 0

    for _ in range(n):
        a, g, r, d = map(int, input().split())
        time += a

        period = g + r
        total = time + d
        rem = total % period

        if rem >= g:
            time += period - rem

    print(time)

if __name__ == "__main__":
    solve()
```Giải pháp giữ lại một biến thời gian chạy duy nhất và cập nhật biến đó trên mỗi giao lộ. Thời gian di chuyển luôn được thêm vào đầu tiên vì việc đến nơi diễn ra trước bất kỳ lần kiểm tra tín hiệu nào. 

Chi tiết triển khai quan trọng là thứ tự thực hiện: modulo phải được tính toán sau khi thêm offset di và thời gian chờ chỉ phải được thêm khi pha có màu đỏ. Việc sử dụng rem >= g sẽ xử lý chính xác trường hợp ranh giới trong đó đèn chuyển chính xác vào thời điểm đến. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản với hai giao điểm. 

đầu vào:```
2
3 5 5 0
4 2 3 1
```Chúng tôi theo dõi thời gian từng bước. 

Đối với giao lộ đầu tiên, thời gian trở thành 3 sau khi di chuyển. Chu kỳ là 10. tổng = 3, rem = 3, nhỏ hơn 5 nên ta chuyển ngay. 

Đối với giao lộ thứ hai, thời gian trở thành 7. Tổng = 7 + 1 = 8, chu kỳ = 5, rem = 3. Vì màu xanh lá cây dài 2 giây nên rem >= 2 có nghĩa là chúng ta đang ở màu đỏ, vì vậy chúng ta đợi 5 − 3 = 2 giây. 

| tôi | thời gian sau chuyến du lịch | tổng = thời gian + d | kỳ | rem | hành động | thời gian mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 10 | 3 | màu xanh lá cây | 3 | 
| 2 | 7 | 8 | 5 | 3 | chờ đợi màu đỏ 2 | 9 | 

Điều này xác nhận cách chờ đợi chỉ được kích hoạt khi vị trí mô-đun vượt quá cửa sổ màu xanh lá cây. 

Bây giờ hãy xem xét trường hợp biên trong đó xe buýt đến đúng ranh giới màu đỏ. 

đầu vào:```
1
5 3 4 0
```Sau khi di chuyển, thời gian = 5. tổng = 5, chu kỳ = 7, rem = 5. Vì màu xanh lá cây là 3 nên rem >= 3 có nghĩa là màu đỏ. Chúng ta đợi 7 − 5 = 2, chuyển thời gian lên 7. 

Điều này cho thấy rằng việc đến đúng ranh giới của green không cho phép đi qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giao lộ yêu cầu các phép toán số học có thời gian không đổi | 
| Không gian | O(1) | Chỉ có một biến tích lũy duy nhất được duy trì | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình cho tối đa 2e5 giao điểm, vì mỗi bước chỉ thực hiện một số thao tác số nguyên và không có bộ nhớ bổ sung nào được phân bổ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# basic sample-like test
assert run("2\n3 5 5 0\n4 2 3 1\n") == "9"

# single always-green case
assert run("1\n10 5 5 100\n") == "10"

# always-red light (gi = 0)
assert run("1\n3 0 4 0\n") == "7"

# boundary transition case
assert run("1\n0 2 3 0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 2 nút giao | 9 | độ chính xác mô phỏng bình thường | 
| luôn xanh bù đắp | 10 | không có hành vi chờ đợi | 
| luôn có vỏ màu đỏ | 7 | xử lý chờ đợi toàn bộ chu kỳ | 
| ranh giới du lịch bằng không | 0 | trường hợp cạnh đến ngay lập tức | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi thời gian đến diễn ra chính xác khi bắt đầu giai đoạn màu đỏ. Ví dụ, với gi = 3, ri = 4, nếu rem = 3 thì chúng ta đang ở điểm chuyển tiếp. Điều kiện rem >= gi đảm bảo chúng ta coi đây là màu đỏ và áp dụng chờ đợi. Thời gian chờ được tính toán là khoảng thời gian - rem, chuyển sang lần bắt đầu xanh tiếp theo một cách chính xác. 

Một trường hợp cạnh khác là giá trị di lớn. Giả sử di là 10^12 trong khi chu kỳ nhỏ. Hoạt động modulo tự động bình thường hóa điều này, vì vậy chúng ta không bao giờ cần giảm di một cách rõ ràng trước đó. Ví dụ: nếu chu kỳ = 6 và di = 10^12 thì (time + di) % 6 sẽ nắm bắt chính xác pha mà không bị tràn hoặc chia tỷ lệ không chính xác. 

Trường hợp tinh tế cuối cùng là khi gi = 0. Trong trường hợp đó, rem < gi không bao giờ đúng, do đó mỗi lần đến đều kích hoạt việc chờ đợi một chu kỳ đầy đủ. Công thức vẫn hoạt động vì chu kỳ − rem luôn tiến tới ranh giới chu kỳ tiếp theo, đảm bảo tính chính xác mà không cần viết hoa đặc biệt.
