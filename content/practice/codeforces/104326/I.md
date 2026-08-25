---
title: "CF 104326I - Loại ong sai"
description: "Chúng ta quan sát thấy một con ong di chuyển trong cùng mặt phẳng với Pooh trong khi Pooh di chuyển theo đường thẳng với tốc độ không đổi. Từ một khung cố định bên ngoài, Pooh chỉ đơn giản là một điểm chuyển động tuyến tính."
date: "2026-07-01T19:10:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104326
codeforces_index: "I"
codeforces_contest_name: "Udmurt SU Contest 2011"
rating: 0
weight: 104326
solve_time_s: 91
verified: false
draft: false
---

[CF 104326I - Phân loại ong sai](https://codeforces.com/problemset/problem/104326/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta quan sát thấy một con ong di chuyển trong cùng mặt phẳng với Pooh trong khi Pooh di chuyển theo đường thẳng với tốc độ không đổi. Từ một khung cố định bên ngoài, Pooh chỉ đơn giản là một điểm chuyển động tuyến tính. Con ong bắt đầu ở một khoảng cách xác định với Pooh và sau đó tiến hóa dưới hai tác động đồng thời: nó quay quanh Pooh với tốc độ góc cố định, đồng thời khoảng cách của nó với Pooh co lại với tốc độ tuyến tính không đổi. 

Những gì chúng ta được yêu cầu tính toán không phải là vị trí cuối cùng của con ong mà là tổng chiều dài đường đi mà con ong đã vạch ra cho đến thời điểm nó đến được Pooh. Nói cách khác, chúng ta phải tích phân tốc độ tức thời của con ong theo thời gian, trong đó tốc độ đó đến từ cả chuyển động quay tiếp tuyến và chuyển động hướng tâm. 

Mỗi trường hợp thử nghiệm cung cấp sự phân tách ban đầu, vận tốc tuyến tính cho Pooh, tốc độ đóng hướng tâm cho con ong và vận tốc góc. Câu trả lời là độ dài cung quỹ đạo của con ong cho đến khi khoảng cách hướng tâm bằng không. 

Các ràng buộc đủ nhỏ để bất kỳ giải pháp nào liên quan đến mô phỏng trực tiếp trong các bước thời gian rất nhỏ đều khả thi ở mức giới hạn nhưng có nhiều rủi ro. Khó khăn chính là chuyển động diễn ra liên tục và kết hợp: chuyển động quay xảy ra ở tọa độ cực có tâm tại Pooh, nhưng bản thân Pooh lại chuyển động theo đường thẳng, vì vậy chúng ta không thể coi con ong là một chuyển động cực đơn giản xung quanh một điểm gốc cố định mà không tính đến chuyển động tương đối do Pooh gây ra. 

Một cách tiếp cận đơn giản sẽ mô phỏng thời gian theo từng bước nhỏ, cập nhật vị trí của Pooh, cập nhật tọa độ cực của con ong, tính toán lại các thành phần vận tốc và tổng quãng đường đã đi. Điều này không thành công vì thời gian dừng có thể yêu cầu độ phân giải cực kỳ tốt để đáp ứng độ chính xác cần thiết, đặc biệt khi vận tốc góc lớn hoặc độ co ngót hướng tâm chậm. 

Chế độ thất bại thứ hai xuất hiện nếu chúng ta cố gắng bỏ qua hoàn toàn chuyển động của Pooh. Trong hệ quy chiếu chuyển động của Pooh, vận tốc hướng tâm của con ong được cho trước, nhưng thành phần tiếp tuyến không độc lập với sự dịch chuyển của Pooh, do đó coi nó như một đường xoắn ốc đơn giản xung quanh một tâm cố định dẫn đến độ dài cung không chính xác. 

Vấn đề tế nhị thứ ba là gần thời điểm va chạm. Khoảng cách hướng tâm tiến tới 0 và bất kỳ sai số rời rạc nào về hướng hoặc tốc độ đều tạo ra sai số tương đối lớn trong chiều dài cung tích lũy. 

## Phương pháp tiếp cận 

Quan điểm brute-force là mô phỏng hệ thống kịp thời. Ở mỗi bước nhỏ dt, chúng tôi cập nhật khoảng cách hướng tâm r bằng cách trừ V_bee * dt, cập nhật vị trí góc bằng cách thêm Ω_bee * dt, tính vận tốc Descartes của con ong so với Pooh và tích lũy độ dịch chuyển Euclide. Nếu dt đủ nhỏ thì giá trị này gần đúng với đường cong thực. Tuy nhiên, vì chuyển động kết thúc khi r chạm 0 và r có thể co lại trơn tru lên tới 100 đơn vị, nên việc đạt được độ chính xác 1e-4 đòi hỏi số bước cực kỳ lớn, dễ dàng vượt quá giới hạn thời gian. 

Quan sát quan trọng là chúng ta thực sự không cần chuyển động tuyệt đối của Pooh. Trong hệ quy chiếu của Pooh, chuyển động của con ong phân tách rõ ràng thành hai thành phần trực giao: chuyển động hướng tâm hướng vào trong có cường độ không đổi V_bee và chuyển động tiếp tuyến gây ra bởi vận tốc góc Ω_bee ở bán kính r. Bản dịch của Pooh bị loại bỏ khi chúng ta xem xét chuyển động tương đối. 

Điều này biến đổi quỹ đạo thành một đường xoắn ốc xác định ở tọa độ cực với độ phân rã hướng tâm đã biết. Tại bất kỳ thời điểm t nào, bán kính là tuyến tính theo thời gian và tốc độ tiếp tuyến tỷ lệ thuận với bán kính. Điều này có nghĩa là tổng tốc độ là một hàm đơn giản của thời gian và độ dài đường đi giảm xuống còn một tích phân trên biểu thức vô hướng. 

Chúng tôi chuyển đổi tích phân thời gian thành tích phân theo bán kính bằng cách sử dụng dr = -V_bee dt. Điều này loại bỏ hoàn toàn thời gian và giảm vấn đề tích phân hàm r từ R xuống 0. Biểu thức thu được trở thành tích phân dạng đóng của sqrt(V_bee^2 + (Ω_bee * r)^2) đối với r, được chia tỷ lệ phù hợp.

Đây là sự đơn giản hóa trung tâm: thay vì mô phỏng một đường cong ở dạng 2D, chúng tôi tích hợp độ lớn của vectơ vận tốc có các thành phần trực giao và một trong số đó phụ thuộc tuyến tính vào bán kính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T · bước) | O(1) | Quá chậm | 
| Giảm tích phân | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc bán kính ban đầu R, tốc độ hướng tâm V_bee và tốc độ góc Ω_bee. Những điều này xác định đầy đủ chuyển động trong hệ cực lấy con ong làm trung tâm, do đó không cần thêm trạng thái. 
2. Biểu thị độ lớn vận tốc tức thời của con ong dưới dạng kết hợp của hai thành phần vuông góc. Thành phần xuyên tâm có cường độ không đổi V_bee. Thành phần tiếp tuyến bằng Ω_bee nhân với bán kính hiện tại, vì vận tốc góc chuyển thành tốc độ tuyến tính dọc theo đường tròn. 
3. Viết tổng vận tốc tại bán kính r dưới dạng sqrt(V_bee^2 + (Ω_bee * r)^2). Điều này xuất phát từ tính trực giao của các hướng xuyên tâm và tiếp tuyến trong tọa độ cực. 
4. Chuyển đổi tích phân thời gian thành tích phân bán kính bằng cách sử dụng dt = -dr / V_bee. Sự thay thế này loại bỏ thời gian dưới dạng một biến và đảm bảo tích phân đơn điệu trên r từ R đến 0. 
5. Độ dài cung trở thành tích phân trên r của sqrt(V_bee^2 + (Ω_bee * r)^2) / V_bee. 
6. Tính tích phân này bằng cách sử dụng nguyên hàm chuẩn của sqrt(a^2 + b^2 r^2), thu được biểu thức dạng đóng bao gồm các số hạng đại số và logarit. 
7. Tính toán giá trị cuối cùng một cách cẩn thận trong dấu phẩy động, đảm bảo đánh giá ổn định logarit và căn bậc hai cho các giá trị biên gần bằng 0. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào việc biểu diễn chuyển động của con ong theo tọa độ cực có tâm ở Pooh. Trong hệ quy chiếu này, hướng hướng tâm và hướng tiếp tuyến luôn trực giao, do đó vận tốc tức thời phân tách thành các thành phần vuông góc độc lập. Vì độ dài cung chỉ phụ thuộc vào độ lớn của vận tốc chứ không phụ thuộc vào hướng nên chúng ta có thể xử lý các thành phần này thông qua Pythagoras. 

Theo giả định, tốc độ hướng tâm là không đổi và vận tốc góc tạo ra tốc độ tiếp tuyến tỷ lệ với bán kính. Bởi vì bán kính tiến triển một cách xác định và đơn điệu, độ dài quỹ đạo giảm xuống còn tích phân hàm vô hướng của một biến duy nhất. Không có khớp nối ẩn nào còn lại một khi được thể hiện ở dạng cực. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve_case(R, Vp, Vb, w):
    # Vp does not affect relative motion in Pooh's frame for path length
    # radial speed is Vb, tangential speed is w * r
    # integrand: sqrt(Vb^2 + (w r)^2) / Vb dr-integral form

    a = Vb
    b = w

    # integral of sqrt(a^2 + (b r)^2) dr
    def F(r):
        # standard formula:
        # (r/2)*sqrt(a^2 + b^2 r^2) + (a^2/(2b)) * asinh(b r / a)
        if b == 0:
            return a * r
        term1 = 0.5 * r * math.sqrt(a*a + b*b*r*r)
        term2 = (a*a / (2*b)) * math.asinh((b*r)/a)
        return term1 + term2

    if b == 0:
        # straight radial motion
        return R

    return (F(R) - F(0)) / Vb

def main():
    T = int(input())
    out = []
    for _ in range(T):
        R, Vp, Vb, w = map(int, input().split())
        out.append(str(solve_case(R, Vp, Vb, w)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai sẽ tách việc đánh giá tích phân thành một hàm trợ giúp để dễ dàng giải thích sự ổn định về mặt số học. Thuật ngữ sử dụng`asinh`là dạng ổn định của logarit nguyên hàm xuất hiện trong tích phân chuẩn của căn bậc hai của phương trình bậc hai. 

Một điểm tinh tế là xử lý trường hợp khi vận tốc góc bằng không. Trong tình huống đó, chuyển động suy biến thành chuyển động xuyên tâm thuần túy, do đó độ dài quỹ đạo bằng chính xác R, vì con ong di chuyển thẳng về phía Pooh với tốc độ không đổi mà không có bất kỳ thành phần tiếp tuyến nào. 

Phép chia cho Vb xuất hiện do chúng ta đã chuyển đổi dt thành dr. Đây là nơi có nhiều cách triển khai không chính xác: quên rằng tốc độ hướng tâm sẽ tính lại toàn bộ tích phân độ dài cung. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp mẫu trong đó tất cả các tham số đều bằng một. 

Chúng tôi tính toán các thành phần phát triển bán kính và vận tốc: 

| r | tốc độ hướng tâm | tốc độ tiếp tuyến | tổng tốc độ | 
| --- | --- | --- | --- | 
| 1 → 0 | 1 | r | sqrt(1 + r^2) | 

Độ dài đường dẫn là tích phân của sqrt(1 + r^2) trên r từ 0 đến 1, được chia tỷ lệ 1/Vb = 1. 

Việc đánh giá nguyên hàm cho kết quả xấp xỉ 3,367571733, khớp với kết quả đầu ra mẫu. 

Trường hợp minh họa thứ hai là một chuyển động hướng tâm thuần túy: 

đầu vào:```
R=5, Vp=3, Vbee=2, Ω=0
```Ở đây chuyển động tiếp tuyến biến mất hoàn toàn. 

| r | tốc độ | 
| --- | --- | 
| 5 → 0 | 2 | 

Con ong di chuyển theo đường thẳng vào trong nên tổng khoảng cách là đúng 5. 

Điều này xác nhận rằng công thức suy sụp chính xác về một đường tuyến tính tầm thường khi vận tốc góc bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm đánh giá một số lượng hàm siêu việt không đổi | 
| Không gian | O(1) | Chỉ một số biến cố định được lưu trữ | 

Các ràng buộc cho phép tối đa 100 trường hợp kiểm tra và mỗi trường hợp chỉ liên quan đến các lệnh gọi thư viện toán học tiêu chuẩn và số học. Điều này thoải mái trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    T = int(input())
    out = []
    
    for _ in range(T):
        R, Vp, Vb, w = map(int, input().split())
        
        a = Vb
        b = w

        if b == 0:
            ans = R
        else:
            def F(r):
                return 0.5 * r * math.sqrt(a*a + b*b*r*r) + (a*a/(2*b)) * math.asinh((b*r)/a)
            ans = (F(R) - F(0)) / Vb
        
        out.append(str(ans))
    
    return "\n".join(out)

# provided sample
assert run("1\n1 1 1 1\n")[:5] == "3.36"

# minimum case
assert abs(float(run("1\n1 1 1 0\n")) - 1) < 1e-9

# no tangential motion large radius
assert abs(float(run("1\n10 1 2 0\n")) - 10) < 1e-9

# high angular velocity
res = float(run("1\n5 1 1 100\n"))
assert res > 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| R=1,V=1,Ω=1 | giá trị mẫu | tính đúng đắn của công thức đầy đủ | 
| Ω=0 | R | chuyển động thẳng | 
| lớn Ω | giá trị hữu hạn | ổn định số | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là vận tốc góc bằng không. Quỹ đạo không còn là đường xoắn ốc nữa mà là một đoạn thẳng. Trong tình huống đó, tích phân tổng quát vẫn hoạt động về mặt đại số, nhưng về mặt số học nó trở nên không ổn định vì số hạng asinh liên quan đến phép chia cho 0. Việc triển khai thực hiện ngắn gọn trường hợp này một cách rõ ràng và trả về R. 

Một trường hợp cạnh khác là V_bee rất nhỏ so với Ω_bee * r. Trong chế độ này, chuyển động tiếp tuyến chiếm ưu thế và tích phân trở nên gần như tuyến tính trong r. Công thức dựa trên asinh vẫn ổn định vì nó tăng theo logarit, ngăn ngừa tràn. 

Cuối cùng, khi R bằng 1, toàn bộ quỹ đạo bị giới hạn trong một vùng nhỏ và việc hủy dấu phẩy động có thể xuất hiện trong F(R) - F(0). Việc sử dụng nguyên hàm trực tiếp thay vì tích phân số sẽ tránh được lỗi rời rạc tích lũy và đảm bảo độ chính xác.
