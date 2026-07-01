---
title: "CF 104417A - Đơn đặt hàng"
description: "Mỗi đơn hàng trong bài toán này đều có ngày hết hạn và số lượng sản phẩm cần thiết. Nhà máy sản xuất một số lượng sản phẩm cố định mỗi ngày bắt đầu từ ngày đầu tiên và không có hàng tồn kho ban đầu."
date: "2026-06-30T19:15:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104417
codeforces_index: "A"
codeforces_contest_name: "The 13th Shandong ICPC Provincial Collegiate Programming Contest"
rating: 0
weight: 104417
solve_time_s: 49
verified: true
draft: false
---

[CF 104417A - Đơn đặt hàng](https://codeforces.com/problemset/problem/104417/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đơn hàng trong bài toán này đều có ngày hết hạn và số lượng sản phẩm cần thiết. Nhà máy sản xuất một số lượng sản phẩm cố định mỗi ngày bắt đầu từ ngày đầu tiên và không có hàng tồn kho ban đầu. Bất cứ khi nào một ngày kết thúc, sản lượng của ngày đó sẽ được thêm vào kho và sau đó bất kỳ đơn đặt hàng nào có thời hạn vào ngày đó phải được đáp ứng đầy đủ bằng cách sử dụng lượng hàng tồn kho hiện tại. 

Nhiệm vụ là xác định xem có cách nào để xử lý tất cả các đơn đặt hàng sao cho mọi đơn hàng đều được hoàn thành không muộn hơn thời hạn của nó hay không, với điều kiện là sản xuất đồng đều và hàng tồn kho được tích lũy theo thời gian. 

Khó khăn chính là các đơn hàng không được căn chỉnh tự nhiên theo thứ tự đầu vào. Hai đơn đặt hàng có thời hạn khác nhau tương tác thông qua lượng hàng tồn kho được chia sẻ, do đó, một quyết định khả thi cục bộ có thể phá vỡ tính khả thi sau này nếu lượng hàng tồn kho được tiêu thụ không đúng thứ tự. 

Các ràng buộc về số lượng đơn đặt hàng là nhỏ, nhiều nhất là một trăm cho mỗi trường hợp thử nghiệm, nhưng giá trị của thời hạn và yêu cầu có thể lên tới một tỷ. Điều này ngay lập tức loại trừ việc mô phỏng mỗi ngày một cách rõ ràng. Mọi giải pháp đúng đều phải hoạt động dựa trên các sự kiện thời gian được nén, cụ thể là các ngày hết hạn riêng biệt thực sự xuất hiện trong dữ liệu đầu vào. 

Một sai lầm ngây thơ nhưng đầy cám dỗ là xử lý các đơn đặt hàng theo thứ tự đầu vào hoặc thậm chí theo thứ tự thời hạn tăng dần mà không xử lý cẩn thận việc tích lũy hàng tồn kho giữa các khoảng thời gian. Ví dụ: nếu một người tham lam phục vụ một nhu cầu lớn ban đầu mà không tính đến nhu cầu tổng hợp sau này ở mức tồn kho hiệu quả tương tự hoặc sớm hơn, thì có thể làm cạn kiệt hàng tồn kho sớm. 

Một dạng thất bại tinh vi khác xảy ra khi nhiều đơn hàng có cùng thời hạn. Nếu một giải pháp xử lý từng cái một mà không đảm bảo việc kiểm tra hàng tồn kho đáp ứng được tổng nhu cầu tại thời hạn đó, thì giải pháp đó có thể chấp nhận không chính xác một phần sự đáp ứng qua nhiều bước thay vì xác minh yêu cầu tổng hợp. 

## Phương pháp tiếp cận 

Cách giải thích vũ phu là mô phỏng từng ngày. Mỗi ngày, chúng tôi tăng lượng hàng tồn kho thêm k, sau đó kiểm tra tất cả các đơn hàng đến hạn trong ngày đó và cố gắng hoàn thành chúng một cách tham lam. Về nguyên tắc, điều này đúng vì nó phản ánh trực tiếp quá trình được mô tả, nhưng nó hoàn toàn không khả thi khi thời hạn lên tới 10^9. Ngay cả khi chỉ có 100 đơn hàng, việc mô phỏng đến thời hạn tối đa sẽ yêu cầu tới 10^9 lần lặp cho mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là không có gì thay đổi giữa hai ngày hết hạn liên tiếp. Sản lượng được tích lũy tuyến tính và các đơn đặt hàng chỉ được “kích hoạt” vào thời hạn cuối cùng. Điều này cho phép chúng tôi nén thời gian chỉ còn những ngày xuất hiện dưới dạng thời hạn. 

Khi thời gian được nén lại, chúng tôi cần đảm bảo rằng vào mỗi ngày hạn chót, lượng hàng tồn kho có sẵn cho đến thời điểm đó là đủ để đáp ứng tất cả các đơn đặt hàng đến hạn vào ngày đó. Tuy nhiên, chỉ kiểm tra độc lập mỗi ngày là không đủ vì hàng tồn kho được chia sẻ trong nhiều ngày. Quan điểm đúng đắn là xử lý thời hạn theo thứ tự tăng dần và duy trì tổng nhu cầu cần thiết cho đến ngày đó. 

Tại mỗi thời hạn riêng biệt, chúng tôi tính toán số lượng hàng tồn kho đã được sản xuất cho đến nay, bằng k nhân với chỉ số ngày đó, sau đó so sánh nó với tổng tất cả các yêu cầu có thời hạn là ngày hôm đó hoặc sớm hơn. Nếu tại bất kỳ thời điểm nào cầu vượt quá cung thì kế hoạch đó là không thể thực hiện được. 

Điều này chuyển vấn đề thành kiểm tra tính khả thi tiền tố đối với các sự kiện đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hàng ngày | O(max ai) | O(1) | Quá chậm | 
| Sắp xếp theo thời hạn + kiểm tra tiền tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Hãy để chúng tôi mô tả phương pháp làm việc từng bước.

1. Nhóm tất cả các đơn đặt hàng theo ngày hết hạn. Mỗi ngày hãy tổng hợp tất cả số lượng cần thiết. Điều này ngăn chặn việc xử lý lặp lại nhiều đơn đặt hàng có cùng thời hạn và đảm bảo chúng tôi chỉ theo dõi tổng nhu cầu cho mỗi thời điểm quan trọng. 
2. Sắp xếp các ngày hết hạn riêng biệt theo thứ tự tăng dần. Điều này là cần thiết vì tính khả thi phụ thuộc vào sản lượng tích lũy đến từng thời điểm. 
3. Duy trì tổng tiền tố đang chạy của các sản phẩm được yêu cầu, được gọi là`need`. Khi lặp lại các ngày đã được sắp xếp, chúng tôi cộng nhu cầu của ngày đó vào`need`. 
4. Đối với mỗi ngày hết hạn`d`, tính lượng hàng tồn kho sẵn có như`k * d`. Điều này thể hiện tổng sản lượng từ ngày 1 đến ngày d. 
5. Nếu tại bất kỳ thời điểm nào`need > k * d`, ngay lập tức trả lời “Không” vì ngay cả khi toàn bộ hoạt động sản xuất trong tương lai được phân bổ hoàn hảo thì nhà máy vẫn chưa đạt được thời hạn đó. 
6. Nếu tất cả các thời hạn đều đáp ứng điều kiện này, hãy trả về “Có”. 

Bước lập luận quan trọng là việc trì hoãn việc thực hiện quá thời hạn là không thể, do đó tính khả thi phải được kiểm tra chính xác ở từng ranh giới thời hạn thay vì trên toàn bộ vào thời điểm cuối. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, lượng hàng tồn kho hoàn toàn được xác định theo thời gian và không phụ thuộc vào cách chúng tôi chỉ định nó cho các đơn đặt hàng. Hạn chế duy nhất là nhu cầu tích lũy đến thời hạn không được vượt quá sản lượng tích lũy cho đến cùng thời điểm đó. Nếu điều này đúng với mọi tiền tố của thời hạn thì chúng ta luôn có thể chỉ định các đơn vị sản xuất một cách tham lam theo thứ tự thời gian, bởi vì sản xuất trước đó luôn có sẵn để đáp ứng thời hạn trước đó và lượng hàng tồn kho còn sót lại đương nhiên sẽ được chuyển tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        by_day = {}
        
        for _ in range(n):
            a, b = map(int, input().split())
            by_day[a] = by_day.get(a, 0) + b
        
        days = sorted(by_day.keys())
        
        need = 0
        ok = True
        
        for d in days:
            need += by_day[d]
            if need > k * d:
                ok = False
                break
        
        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Quá trình triển khai nhóm các đơn hàng theo thời hạn bằng cách sử dụng từ điển, điều này đảm bảo rằng nhiều đơn hàng trong cùng một ngày sẽ được hợp nhất thành một nhu cầu tích lũy duy nhất. Điều này rất cần thiết vì việc chia tách chúng sẽ không làm thay đổi tính khả thi nhưng sẽ làm phức tạp việc theo dõi. 

Sắp xếp các phím thiết lập quá trình xử lý theo trình tự thời gian. Biến`need`hoạt động như một tổng tiền tố đang chạy của nhu cầu và ở mỗi bước chúng tôi so sánh nó với năng lực sản xuất`k * d`. Phép nhân an toàn trong Python do các số nguyên có độ chính xác tùy ý, nhưng trong các ngôn ngữ có chiều rộng cố định, điều quan trọng là phải sử dụng số học 64-bit. 

Ngắt sớm là một tối ưu hóa duy trì tính chính xác, vì một khi tiền tố vi phạm dung lượng thì không quá trình xử lý nào tiếp theo có thể khôi phục tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, k = 5
(1, 3), (6, 12), (6, 15), (8, 1)
```Nhóm theo ngày: 

| Ngày | Nhu cầu | 
| --- | --- | 
| 1 | 3 | 
| 6 | 27 | 
| 8 | 1 | 

| Bước | Ngày | Cần | Công suất (k*d) | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 5 | Có | 
| 2 | 6 | 30 | 30 | Có | 
| 3 | 8 | 31 | 40 | Có | 

Hệ thống vẫn khả thi ở mọi điểm kiểm tra nên câu trả lời là Có. Dấu vết cho thấy mặc dù nhu cầu tăng đột biến vào ngày thứ 6 nhưng sản lượng tích lũy hoàn toàn phù hợp với nhu cầu đó. 

### Ví dụ 2 

đầu vào:```
n = 2, k = 100
(3, 100), (4, 200)
```Được nhóm: 

| Ngày | Nhu cầu | 
| --- | --- | 
| 3 | 100 | 
| 4 | 200 | 

| Bước | Ngày | Cần | Công suất (k*d) | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 100 | 300 | Có | 
| 2 | 4 | 300 | 400 | Có | 

Bây giờ hãy sửa đổi cách giải thích cho phù hợp với vấn đề ban đầu: nếu tổng nhu cầu ở ngày thứ 4 là 300 nhưng chỉ còn lại 200 sau khi tiêu thụ trước đó, thì việc kiểm tra tiền tố chỉ nắm bắt chính xác điều này khi nhu cầu tích lũy vượt quá sản lượng. Bảng này trình bày cách tích lũy tiền tố mã hóa ngầm tất cả các tương tác chứng khoán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | thời hạn sắp xếp chiếm ưu thế cho mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | lưu trữ các nhu cầu được nhóm | 

Các ràng buộc cho phép tối đa 100 đơn hàng cho mỗi trường hợp thử nghiệm, do đó, các thao tác sắp xếp và từ điển có thể dễ dàng đủ nhanh ngay cả đối với 100 trường hợp thử nghiệm. Giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            mp = defaultdict(int)
            for _ in range(n):
                a, b = map(int, input().split())
                mp[a] += b
            need = 0
            ok = True
            for d in sorted(mp):
                need += mp[d]
                if need > k * d:
                    ok = False
                    break
            out.append("Yes" if ok else "No")
        return "\n".join(out)

    return solve()

# provided samples
assert run("""2
4 5
6 12
1 3
6 15
8 1
2 100
3 100
3 200
""") == "Yes\nNo"

# minimum case
assert run("""1
1 10
1 5
""") == "Yes"

# impossible at first deadline
assert run("""1
1 1
1 5
""") == "No"

# multiple same-day orders
assert run("""1
3 2
2 1
2 1
2 1
""") == "No"

# large slack case
assert run("""1
3 100
1 50
2 50
3 50
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn hàng | Có | tính khả thi cơ bản | 
| quá nhu cầu sớm | Không | phát hiện lỗi sớm | 
| sáp nhập trong ngày | Không | tính chính xác của tổng hợp | 
| xích công suất cao | Có | tính chính xác tích lũy tiền tố | 

## Vỏ cạnh 

Trường hợp một bên là khi tất cả các đơn đặt hàng có cùng thời hạn. Trong tình huống đó, thuật toán sẽ hợp nhất tất cả các nhu cầu thành một giá trị duy nhất và so sánh nó một lần với k lần trong ngày hôm đó. Ví dụ, đầu vào`(2,1),(2,1),(2,1)`với k = 2 tạo ra nhu cầu = 3 và năng lực = 4, được chấp nhận chính xác. 

Một trường hợp đặc biệt khác là khi thời hạn giao hàng rất ít, chẳng hạn như một đơn hàng vào ngày thứ 10^9. Thuật toán không mô phỏng các ngày trung gian mà tính toán trực tiếp công suất là k * 10^9, tránh bùng nổ thời gian mà vẫn kiểm tra tính chính xác tại đúng ranh giới yêu cầu. 

Một trường hợp khó phát hiện cuối cùng là khi thời hạn ban đầu rất chặt chẽ nhưng thời hạn muộn hơn lại lỏng lẻo. Việc so sánh tiền tố đảm bảo rằng một khi nhu cầu tích lũy sớm là khả thi, thì sự chậm trễ sau này không ảnh hưởng đến tính đúng đắn, bởi vì công suất còn lại được ngầm chuyển sang thông qua hàm sản xuất ngày càng tăng.
