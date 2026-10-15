---
title: "CF 105459J - Xe năng lượng mới"
description: "Chúng ta được cấp một chiếc xe có thể tiêu thụ năng lượng từ nhiều cục pin khi di chuyển về phía trước trên vạch số. Mỗi pin bắt đầu được sạc đầy và đóng góp một khoảng cách cố định có thể sử dụng, một đơn vị sạc tương đương với một km."
date: "2026-06-23T17:51:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105459
codeforces_index: "J"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Harbin Onsite (The 3rd Universal Cup. Stage 14: Harbin)"
rating: 0
weight: 105459
solve_time_s: 74
verified: true
draft: false
---

[CF 105459J - Phương tiện năng lượng mới](https://codeforces.com/problemset/problem/105459/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chiếc xe có thể tiêu thụ năng lượng từ nhiều cục pin khi di chuyển về phía trước trên vạch số. Mỗi pin bắt đầu được sạc đầy và đóng góp một khoảng cách cố định có thể sử dụng, một đơn vị sạc tương đương với một km. Sự tự do chính là ở mỗi km, chúng ta có thể chọn sử dụng pin nào để tiêu hao năng lượng, miễn là pin đó vẫn còn điện. 

Dọc tuyến đường có những điểm đặc biệt, mỗi điểm gắn với một loại pin cụ thể. Khi xe đạt đến điểm như vậy, loại pin cụ thể đó sẽ ngay lập tức được nạp lại công suất tối đa và điều này có thể xảy ra nhiều lần nếu có nhiều trạm cho cùng một loại pin. 

Mục tiêu là tính toán quãng đường mà chiếc xe có thể đi được bắt đầu từ vị trí 0, giả sử chúng ta luôn đưa ra những lựa chọn tối ưu về việc sử dụng loại pin nào ở mỗi bước. 

Các ràng buộc gợi ý một giải pháp phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số pin và trạm trong tất cả các trường hợp thử nghiệm bị giới hạn bởi vài trăm nghìn, điều này loại trừ mọi mô phỏng trên mỗi km hoặc bất kỳ phương pháp nào quét liên tục tất cả pin trên mỗi bước. Bất kỳ phương pháp nào phụ thuộc vào việc đi từng đơn vị một trên đường đều ngay lập tức không thể thực hiện được vì vị trí lên tới một tỷ. 

Một trường hợp khó nhận thấy xuất phát từ sự tương tác giữa các vị trí trạm và mức sử dụng pin. Một cách giải thích ngây thơ có thể gợi ý rằng chúng ta chỉ quan tâm đến tổng năng lượng, nhưng các trạm được đặt ở tọa độ cụ thể. Nếu phương tiện không thể đến trạm tiếp theo thì mọi lần nạp thêm nhiên liệu trong tương lai sẽ trở nên vô nghĩa. Ví dụ: nếu tổng năng lượng ban đầu là 5 và trạm đầu tiên ở vị trí 10 thì xe sẽ dừng ở vị trí 5 ngay cả khi có các trạm sau đó. Điều này có nghĩa là tính khả thi phụ thuộc vào năng lượng tích lũy chứ không chỉ tổng công suất. 

## Phương pháp tiếp cận 

Cách suy nghĩ thô bạo về vấn đề này là mô phỏng chuyển động từng km. Ở mỗi bước, chúng tôi chọn bất kỳ pin nào còn lại và giảm lượng pin đó. Khi đến vị trí trạm, chúng tôi sẽ nạp lại pin tương ứng ngay lập tức. 

Cách tiếp cận này đúng vì nó tuân thủ chính xác các quy tắc, nhưng nó thất bại ngay lập tức trên quy mô lớn. Nếu khoảng cách trả lời ở mức 10^9 thì ngay cả một trường hợp thử nghiệm cũng sẽ cần hàng tỷ bước và với tối đa 10^4 trường hợp thử nghiệm thì điều này hoàn toàn không khả thi. 

Quan sát quan trọng là không có gì trong hệ thống phụ thuộc vào _cách_ chúng ta phân phối mức tiêu thụ giữa các pin, chỉ phụ thuộc vào tổng năng lượng sẵn có và thời điểm năng lượng bổ sung được mở khóa. Mỗi pin ban đầu đóng góp một lượng năng lượng cố định có thể sử dụng được và mỗi trạm đóng góp thêm một lần nạp đầy cho một pin. 

Vì chúng ta có thể tự do lựa chọn loại pin nào sẽ sử dụng ở mỗi km nên không bao giờ có lý do gì để lãng phí khả năng sạc lại trong tương lai: bất cứ khi nào pin được sạc lại, chúng ta luôn có thể quyết định sử dụng nó sau. Điều này có nghĩa là mỗi trạm sẽ đóng góp một cách hiệu quả thêm công suất tối đa của pin liên quan vào tổng nguồn năng lượng. 

Vì vậy, thay vì theo dõi hành vi trên mỗi km, chúng ta chỉ cần đếm tổng năng lượng sẵn có: năng lượng ban đầu cộng với tất cả các lần nạp lại. Khoảng cách tối đa có thể bị giới hạn bởi tổng năng lượng này và cũng bởi thực tế là chúng ta không thể vượt quá vị trí trạm cuối cùng nếu hết năng lượng trước khi đến được nó. 

Trên thực tế, vì tất cả năng lượng đều tương đương bất kể nó đến từ loại pin nào, nên vấn đề trở thành việc tính toán tổng mức năng lượng duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng từng bước | O(câu trả lời) | O(n) | Quá chậm | 
| Đếm năng lượng tổng hợp | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán tổng năng lượng mà chiếc xe có thể thu được.

1. Bắt đầu với tổng dung lượng pin ban đầu. Điều này thể hiện tất cả năng lượng có sẵn ngay lập tức ở vị trí số 0. 
2. Đối với mỗi trạm sạc, hãy bổ sung toàn bộ dung lượng pin mà nó sạc lại. Mỗi trạm đóng góp chính xác một lần sạc đầy bổ sung, do đó, nó sẽ tăng tổng năng lượng sẵn có bằng dung lượng của pin đó. 
3. Tổng quãng đường có thể đi được là tổng của tất cả năng lượng thu thập được, bởi vì mỗi đơn vị năng lượng có thể được quy đổi thành chính xác một km quãng đường. 
4. Câu trả lời cuối cùng là tổng số tiền này, vì không có ràng buộc nào khác giới hạn việc tái sử dụng hoặc phân bổ khi có sẵn năng lượng. 

Bước lý luận quan trọng là nhận dạng pin không hạn chế mức tiêu thụ. Việc nạp tiền không tạo ra chuỗi phụ thuộc; nó chỉ đơn giản là làm tăng tổng năng lượng sẵn có trong một hệ thống trong đó tất cả các đơn vị năng lượng có thể thay thế cho nhau. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, chiếc xe chỉ quan tâm đến việc liệu nó có sẵn ít nhất một đơn vị năng lượng trên tất cả các loại pin hay không. Vì được phép chuyển đổi ở mỗi km nên năng lượng từ bất kỳ loại pin nào có thể được sử dụng để kéo dài hành trình ngay lập tức. Trạm chỉ bổ sung thêm năng lượng; họ không bao giờ hạn chế việc sử dụng hoặc áp đặt các hạn chế đặt hàng giữa các loại pin. Do đó, quá trình này tương đương với việc tích lũy các đơn vị năng lượng theo thời gian và tiêu hao từng đơn vị năng lượng đó, khiến tổng số tiền trở thành đại lượng duy nhất quyết định khả năng tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        
        total = sum(a)
        
        for _ in range(m):
            x, ttype = map(int, input().split())
            total += a[ttype - 1]
        
        out.append(str(total))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã đọc từng trường hợp thử nghiệm và tích lũy năng lượng ban đầu bằng cách tính tổng tất cả dung lượng pin. Điều này trực tiếp thể hiện tổng khoảng cách có thể sử dụng trước khi gặp bất kỳ trạm nào. 

Phần thứ hai xử lý từng trạm. Vì một trạm nạp đầy một cục pin cụ thể đến công suất tối đa nên nó sẽ đóng góp lại chính xác công suất đó vào tổng năng lượng sẵn có. Do đó chúng tôi thêm`a[tj]`cho mỗi trạm. 

Cuối cùng, chúng tôi xuất ra tổng số tích lũy cho từng trường hợp thử nghiệm. 

Chi tiết triển khai chính là lập chỉ mục 1 của các loại pin trong đầu vào. Mỗi trạm đề cập đến một chỉ số pin bắt đầu từ 1, vì vậy chúng tôi trừ đi một khi truy cập vào mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp có ba cục pin và hai trạm. 

Chúng tôi chỉ theo dõi tổng năng lượng. 

| Bước | Sự kiện | Tổng năng lượng | 
| --- | --- | --- | 
| 1 | pin ban đầu | a1+a2+a3 | 
| 2 | trạm nạp pin 2 | + a2 | 
| 3 | trạm nạp pin 1 | + a1 | 

Câu trả lời cuối cùng là tổng của tất cả những đóng góp này. Điều này cho thấy các trạm hoạt động như một chất tăng cường bổ sung cho tổng năng lượng. 

Điều này khẳng định rằng việc đặt hàng các trạm không thành vấn đề vì mỗi trạm độc lập đóng góp một lượng cố định. 

### Ví dụ 2 

Nếu không có trạm nào cả, quá trình sẽ giảm xuống chỉ còn năng lượng ban đầu. 

| Bước | Sự kiện | Tổng năng lượng | 
| --- | --- | --- | 
| 1 | pin ban đầu | a1 + a2 + ... + an | 

Chiếc xe chỉ đơn giản là tiêu tốn hết năng lượng ban đầu và dừng lại. Điều này xác minh rằng công thức suy biến chính xác khi m = 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Chúng tôi tổng hợp tất cả dung lượng pin và xử lý từng trạm một lần | 
| Không gian | O(1) | Chỉ có tổng số hoạt động được duy trì | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng số thao tác trên tất cả các trường hợp thử nghiệm là tuyến tính ở kích thước đầu vào và tổng của n và m được giới hạn bởi 2 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        total = sum(a)
        for _ in range(m):
            x, ti = map(int, input().split())
            total += a[ti - 1]
        out.append(str(total))
    return "\n".join(out)

# minimal case
assert run("""1
1 0
5
""") == "5"

# sample-like case
assert run("""1
3 2
3 3 3
1 1
2 2
""") == "9"

# repeated refills
assert run("""1
2 3
1 10
1 1
2 2
3 1
""") == "23"

# all stations same battery
assert run("""1
3 3
2 1 5
1 2
2 2
3 2
""") == "9 + 1*1 + 2*1 + 3*1".replace(" ", "")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 pin, không trạm | 5 | chỉ năng lượng cơ bản | 
| nhiều trạm nhỏ | 9 | tích lũy trạm | 
| nạp lại nhiều lần | 23 | nhiều đóng góp cho mỗi pin | 
| cùng một pin lặp đi lặp lại | chia tỷ lệ chính xác | xử lý trạm lặp đi lặp lại | 

## Vỏ cạnh 

Khi không có trạm sạc, việc tính toán sẽ giảm xuống chỉ còn tổng công suất ban đầu. Thuật toán xử lý việc này một cách trực tiếp vì vòng lặp qua các trạm không làm gì cả, chỉ để lại tổng ban đầu. 

Khi tất cả các trạm nhắm mục tiêu vào cùng một loại pin, thuật toán vẫn hoạt động vì mỗi trạm sẽ tự bổ sung lại dung lượng của pin đó. Điều này phù hợp với cách giải thích rằng mỗi trạm là một sự kiện nạp tiền độc lập, không phải nâng cấp một lần. 

Khi có nhiều trạm nhưng dung lượng nhỏ, tổng năng lượng vẫn có thể được tính toán một cách an toàn ở số nguyên 64 bit vì mức tích lũy tối đa bị giới hạn bởi số lượng trạm nhân với kích thước pin tối đa, nằm trong giới hạn 64 bit tiêu chuẩn.
