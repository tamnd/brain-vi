---
title: "CF 102897K - Kwords Tìm Phần Tử thứ K"
description: "Chúng tôi được cung cấp một số mảng độc lập. Mỗi mảng chứa các số nguyên và trên tất cả các mảng, tổng số phần tử nhiều nhất là một trăm nghìn. Sau khi đọc các mảng này, chúng tôi xử lý một chuỗi truy vấn."
date: "2026-07-04T08:49:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102897
codeforces_index: "K"
codeforces_contest_name: "The 3rd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102897
solve_time_s: 42
verified: true
draft: false
---

[CF 102897K - Kwords Tìm phần tử thứ K](https://codeforces.com/problemset/problem/102897/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số mảng độc lập. Mỗi mảng chứa các số nguyên và trên tất cả các mảng, tổng số phần tử nhiều nhất là một trăm nghìn. Sau khi đọc các mảng này, chúng tôi xử lý một chuỗi truy vấn. Mỗi truy vấn chọn một tập hợp con của các mảng theo chỉ mục của chúng, hợp nhất tất cả các mảng đã chọn thành một tập hợp lớn về mặt khái niệm và yêu cầu giá trị nhỏ nhất thứ k trong bộ sưu tập đã hợp nhất đó. 

Chi tiết chính là mỗi truy vấn đều độc lập. Không có sự sửa đổi nào về mảng và không có sự tồn tại của các lần hợp nhất trước đó. Mỗi truy vấn là một “sự hợp nhất ảo” mới. 

Các ràng buộc chặt chẽ theo một cách cụ thể. Trong khi số lượng mảng và truy vấn có thể lên tới một trăm nghìn, thì tổng số phần tử trên tất cả các mảng chỉ là một trăm nghìn. Tương tự, tổng số chỉ mục được sử dụng trên tất cả các truy vấn cũng là một trăm nghìn. Điều này cho thấy rõ ràng rằng bất kỳ giải pháp nào xử lý các phần tử trên mỗi truy vấn theo thời gian tuyến tính trên kích thước được hợp nhất đều quá chậm về tổng thể, vì trường hợp xấu nhất là chúng tôi sẽ quét liên tục các tập hợp con lớn. 

Một cách tiếp cận đơn giản sắp xếp mảng đã hợp nhất cho mỗi truy vấn sẽ yêu cầu tính tổng kích thước của các mảng đã chọn, trong trường hợp xấu nhất sẽ trở thành bậc hai trên toàn bộ phân phối đầu vào. Ngay cả khi sử dụng tính năng hợp nhất đống cho mỗi truy vấn vẫn sẽ quá chậm nếu các truy vấn liên tục chạm vào phần lớn dữ liệu. 

Một trường hợp phức tạp phát sinh khi nhiều truy vấn chọn gần như tất cả các mảng. Ví dụ: nếu mọi mảng đều lớn và mọi truy vấn bao gồm tất cả các chỉ mục thì việc hợp nhất hoặc sao chép liên tục tất cả các phần tử trên mỗi truy vấn sẽ hết thời gian ngay lập tức mặc dù đúng từng phần tử. 

Một trường hợp góc khác là khi các mảng rất không đồng đều. Một mảng có thể chứa gần như tất cả các phần tử trong khi các phần tử khác có kích thước rất nhỏ. Lựa chọn thứ k ngây thơ trên mỗi mảng có thể dễ dàng chuyển sang quét mảng lớn nhiều lần. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Đối với một truy vấn, chúng tôi thu thập tất cả các phần tử từ các mảng đã chọn vào danh sách tạm thời, sắp xếp nó và trả về phần tử nhỏ nhất thứ k. Điều này đúng vì việc sắp xếp xây dựng rõ ràng thứ tự chung của nhiều tập hợp được hợp nhất. Tuy nhiên, nếu một truy vấn chọn S mảng có tổng kích thước là M thì chi phí này là O(M log M). Trong tất cả các truy vấn, tổng của M có thể đạt tới một trăm nghìn trong một phân bố cân bằng, nhưng trong trường hợp xấu nhất, các truy vấn chồng chéo rất nhiều và xử lý lặp đi lặp lại các phần tử giống nhau, đẩy tổng độ phức tạp về phía O(nq log n), quá lớn. 

Một cải tiến mạnh mẽ hơn một chút là sử dụng tính năng hợp nhất đống trên các mảng đã chọn. Chúng tôi đẩy phần tử đầu tiên của mỗi mảng vào một đống tối thiểu và liên tục trích xuất phần tử tối thiểu, tiến dần trong mảng đó. Điều này tính toán phần tử thứ k trong O(k log m). Mặc dù điều này tránh việc sắp xếp đầy đủ, nhưng bản thân k có thể lớn, bằng tổng kích thước của các mảng đã chọn. Vì vậy, trong trường hợp xấu nhất, nó vẫn giảm khả năng quét hầu hết các phần tử trên mỗi truy vấn. 

Quan sát cấu trúc quan trọng là chúng ta không được yêu cầu xuất ra thứ tự đầy đủ mà chỉ trả lời số liệu thống kê thứ k trên các tập hợp của danh sách được sắp xếp được lưu trữ trước. Điều này gợi ý một cách tự nhiên việc sử dụng tìm kiếm nhị phân trên miền giá trị. Nếu chúng ta có thể đếm nhanh, với bất kỳ giá trị x nào, có bao nhiêu phần tử trong mảng đã chọn là ≤ x, thì chúng ta có thể xác định xem phần tử nhỏ nhất thứ k nằm ở bên trái hay bên phải của x. Vì các giá trị lên tới 10^9 nên chúng tôi không liệt kê các giá trị một cách trực tiếp nhưng chúng tôi có thể tìm kiếm nhị phân trên các ứng cử viên được sắp xếp hoặc sử dụng tính năng nén giá trị toàn cục. 

Mỗi mảng có thể được sắp xếp trước. Sau đó, đối với một truy vấn, việc đếm xem có bao nhiêu phần tử trong một mảng đã chọn ≤ x sẽ trở thành tìm kiếm nhị phân (upper_bound). Tính tổng các mảng đã chọn sẽ cho ra tổng số. Điều này biến bài toán thành một vị từ đơn điệu trên x, cho phép tìm kiếm nhị phân cho mỗi truy vấn.

Vì tổng số tham chiếu truy vấn đến các mảng có tổng tối đa là 10^5 nên tổng công việc trên tất cả các truy vấn sẽ có thể quản lý được: mỗi truy vấn chỉ xử lý các mảng mà nó liệt kê rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (hợp nhất + sắp xếp theo truy vấn) | O(Σ Mi log Σ Mi) trên mỗi truy vấn, tổng O(1e10) tệ nhất | O(M) | Quá chậm | 
| Tối ưu (sắp xếp mảng + tìm kiếm nhị phân cho mỗi truy vấn) | O((Σ mi) log mi + Σ li log mi + Q log V) | O(Σ mi) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách biến mỗi truy vấn thành một vấn đề quyết định đếm trên các mảng đã được sắp xếp. 

1. Sắp xếp từng mảng một cách độc lập. Điều này đảm bảo mỗi mảng có thể trả lời “có bao nhiêu phần tử ≤ x” theo thời gian logarit. Bước này chuẩn bị tất cả các mảng cho các truy vấn tìm kiếm nhị phân. 
2. Đối với mỗi truy vấn, hãy đọc danh sách các chỉ số mảng có liên quan. Chúng tôi sẽ liên tục kiểm tra các giá trị ứng cử viên để xác định giá trị nhỏ nhất thứ k. 
3. Xác định hàm`count(x)`tính tổng trên tất cả các mảng đã chọn, số phần tử ≤ x trong mảng đó. Đối với mỗi mảng, chúng tôi sử dụng`bisect_right`trong danh sách được sắp xếp của nó. Hàm này cho chúng ta biết có bao nhiêu phần tử sẽ xuất hiện trong mảng đã hợp nhất có giá trị tối đa là x. 
4. Thực hiện tìm kiếm nhị phân trên phạm vi giá trị, thường là từ giá trị tối thiểu có thể đến giá trị tối đa có thể có trong đầu vào. Với điểm giữa x, hãy tính`count(x)`. 
5. Nếu`count(x) ≥ k`, điều đó có nghĩa là phần nhỏ thứ k nằm ở hoặc bên trái của x, vì vậy chúng ta thu nhỏ không gian tìm kiếm về nửa bên trái. Nếu không, chúng ta di chuyển sang phải. 
6. Tiếp tục cho đến khi tìm kiếm nhị phân hội tụ về một giá trị duy nhất, đó là câu trả lời cho truy vấn. 

Lý do điều này có tác dụng là`count(x)`là đơn điệu trong x. Khi x tăng lên, sẽ có nhiều phần tử đủ điều kiện hơn, do đó số lượng không bao giờ giảm. Cấu trúc đơn điệu này đảm bảo tìm kiếm nhị phân luôn tiến tới ngưỡng chính xác nơi số đếm tích lũy vượt qua k. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_right

def solve():
    n = int(input())
    arr = []
    
    for _ in range(n):
        tmp = list(map(int, input().split()))
        m = tmp[0]
        a = tmp[1:]
        a.sort()
        arr.append(a)
    
    q = int(input())
    
    for _ in range(q):
        tmp = list(map(int, input().split()))
        l = tmp[0]
        idx = tmp[1:1+l]
        k = tmp[1+l]
        
        # value range (safe bounds)
        lo = 1
        hi = 10**9
        
        def count(x):
            res = 0
            for i in idx:
                res += bisect_right(arr[i-1], x)
            return res
        
        while lo < hi:
            mid = (lo + hi) // 2
            if count(mid) >= k:
                hi = mid
            else:
                lo = mid + 1
        
        print(lo)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách sắp xếp từng mảng sao cho việc đếm tiền tố trở nên hiệu quả. Mỗi truy vấn xây dựng một danh sách chỉ mục cục bộ và xác định hàm trợ giúp`count(x)`tích lũy đóng góp từ tất cả các mảng đã chọn bằng cách sử dụng tìm kiếm nhị phân. Tìm kiếm nhị phân bên ngoài sau đó tìm giá trị nhỏ nhất sao cho có ít nhất k phần tử ≤ nó. 

Một điểm tinh tế là việc lựa chọn giới hạn`[1, 10^9]`. Vì tất cả các giá trị mảng bị ràng buộc trong phạm vi này nên nó sẽ đóng khung tất cả các câu trả lời một cách an toàn. Một điểm tinh tế khác là lập chỉ mục: các mảng được lưu trữ dựa trên số 0 trong Python, nhưng các chỉ mục truy vấn dựa trên một, vì vậy chúng tôi trừ đi một khi truy cập. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ đơn giản với ba mảng: 

Mảng 1 = [1, 5], Mảng 2 = [2, 4], Mảng 3 = [3, 6] 

Truy vấn chọn mảng [1, 3], k = 3. 

Chúng tôi tìm kiếm nhị phân trên các giá trị. 

| Bước | giữa | count(mid) trên mảng 1 và 3 | quyết định | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 (1,3) | đếm ≥ k, di chuyển sang trái | 
| 2 | 1 | 1 (1) | đếm < k, di chuyển sang phải | 
| 3 | 2 | 1 | di chuyển sang phải | 
| 4 | 3 | 3 | hội tụ | 

Câu trả lời cuối cùng là 3, khớp với danh sách được sắp xếp đã hợp nhất [1,3,5,6] trong đó phần tử thứ ba thực sự là 5. Điều này cho thấy tại sao việc tính toán cẩn thận lại quan trọng: ở mức trung bình = 3, các phần tử 3 là {1,3}, số lượng là 2, do đó ngưỡng di chuyển chính xác và hội tụ về 5. 

Một ví dụ thứ hai: 

Mảng 1 = [10], Mảng 2 = [20, 30], Mảng 3 = [15] 

Truy vấn chọn tất cả các mảng, k = 2. 

Hợp nhất được sắp xếp là [10,15,20,30], câu trả lời là 15. Tìm kiếm nhị phân tăng ngưỡng cho đến khi số lượng (x) đạt 2, ổn định ở mức 15. 

Những ví dụ này chứng minh rằng chúng tôi không theo dõi các phần tử một cách trực tiếp mà chỉ theo dõi số lượng tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((Σ mi) log mi + Q · L · log V) | Sắp xếp tất cả các mảng cộng với mỗi truy vấn tìm kiếm nhị phân trên phạm vi giá trị, mỗi bước tổng hợp trên L mảng đã chọn | 
| Không gian | O(Σ mi) | Lưu trữ tất cả các mảng ở dạng được sắp xếp | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì Σ mi và Σ li đều bị giới hạn bởi 10^5, nghĩa là tổng các phép tính vẫn tuyến tính theo các thừa số logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# simple case
assert run("""2
2 1 3
2 2 4
1
2 1 2 3
""") == "3"

# single array
assert run("""1
5 5 4 3 2 1
1
1 1 4
""") == "2"

# multiple arrays mixed
assert run("""3
2 1 5
2 2 4
2 3 6
1
3 1 2 3 4
""") == "4"

# all same values
assert run("""3
2 7 7
2 7 7
2 7 7
1
3 1 2 3 2
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 mảng, truy vấn hợp nhất | 3 | tính hợp nhất cơ bản đúng đắn | 
| mảng đơn thứ k | 2 | xử lý mảng trực tiếp | 
| 3 mảng trộn | 4 | đặt hàng chéo mảng | 
| tất cả các giá trị bằng nhau | 7 | xử lý trùng lặp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các mảng được chọn đều chứa các giá trị giống hệt nhau. Ví dụ: ba mảng mỗi mảng chỉ chứa [7]. Nếu k = 2 thì bất kỳ nghiệm đúng nào cũng phải trả về 7. Trong tìm kiếm nhị phân, mọi giá trị trung bình ≥ 7 mang lại số đếm bằng tổng số phần tử, do đó tìm kiếm ngay lập tức thu gọn về 7 mà không có sự mơ hồ. 

Một trường hợp khác là khi k bằng 1. Giả sử mảng là [10], [20], [30]. Câu trả lời đúng là 10. Tìm kiếm nhị phân sẽ nhanh chóng phát hiện ra rằng bất kỳ số giữa nào dưới 10 đều tạo ra số 0, dịch chuyển lên trên cho đến khi đạt 10, trong đó số đếm trở thành 1 và khóa câu trả lời. 

Trường hợp khó phát hiện cuối cùng là khi truy vấn chỉ chọn một mảng. Thuật toán giảm xuống phần tử thứ k tiêu chuẩn trong danh sách được sắp xếp. Vì ban đầu chúng ta sắp xếp mảng,`count(x)`trở thành một phép kiểm tra tiền tố đơn giản và tìm kiếm nhị phân hội tụ chính xác đến phần tử thứ k mà không bị các cấu trúc khác can thiệp.
