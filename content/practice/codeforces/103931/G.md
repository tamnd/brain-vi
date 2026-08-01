---
title: "CF 103931G - Gua!"
description: "Chúng ta được cung cấp một mẫu vũ khí duy nhất được mô tả bằng hai tham số và bản phát lại của một phân đoạn trận đấu. Loại vũ khí này gây sát thương tối đa $B$ cho mỗi viên đạn và có tốc độ bắn là $R$ phát mỗi phút. Từ đó chúng ta có thể suy ra tần suất đạn có thể được bắn."
date: "2026-07-02T07:17:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103931
codeforces_index: "G"
codeforces_contest_name: "2022 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103931
solve_time_s: 49
verified: true
draft: false
---

[CF 103931G - Gua!](https://codeforces.com/problemset/problem/103931/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mẫu vũ khí duy nhất được mô tả bằng hai tham số và bản phát lại của một phân đoạn trận đấu. Vũ khí gây ra nhiều nhất$B$sát thương trên mỗi viên đạn và có tốc độ bắn là$R$vòng mỗi phút. Từ đó chúng ta có thể suy ra tần suất đạn có thể được bắn. Việc phát lại cho chúng ta biết rằng người chơi đã tích lũy tổng giá trị thiệt hại$D$trong một khoảng thời gian$S$giây, tính từ thời điểm viên đạn đầu tiên được bắn đến thời điểm viên đạn cuối cùng chạm đất. 

Nhiệm vụ không phải là mô phỏng cuộc chiến mà là quyết định xem liệu thiệt hại được báo cáo có thể xảy ra về mặt vật lý hay không dưới các hạn chế bắn của vũ khí. Nếu thiệt hại vượt quá mức mà bất kỳ chuỗi cú đánh hợp lệ nào có thể tạo ra, chúng tôi phải đưa ra kết quả rằng người chơi chắc chắn đang gian lận. 

Hạn chế chính là tốc độ bắn. Nếu như$R > 0$, thời gian tối thiểu giữa hai viên đạn là$60 / R$giây. Điều này ngụ ý số lượng đạn tối đa có thể được bắn trong một khoảng thời gian nhất định. Nếu như$R = 0$, vũ khí hoàn toàn không thể bắn, vì vậy bất kỳ sát thương tích cực nào ngay lập tức ngụ ý gian lận trừ khi sát thương bằng 0. 

Sự tinh tế là cách diễn giải cửa sổ thời gian$S$. Vì nó được định nghĩa là thời gian từ viên đạn đầu tiên đến viên đạn cuối cùng nên số phát bắn$k$thỏa mãn rằng cảnh quay cuối cùng xảy ra vào thời điểm$S$, nhưng phát súng đầu tiên vào đúng thời điểm$0$. Như vậy,$k$đạn yêu cầu$k - 1$khoảng thời gian, mỗi khoảng ít nhất$60 / R$, nghĩa:$$(k - 1) \cdot \frac{60}{R} \le S$$Vì thế:$$k \le \left\lfloor \frac{S \cdot R}{60} \right\rfloor + 1$$Trường hợp cạnh rất quan trọng. Nếu như$R = 0$, sau đó$k = 0$trừ khi không có thiệt hại nào được gây ra. Nếu như$S = 0$, cửa sổ chỉ cho phép bắn một phát duy nhất, vì viên đạn đầu tiên và viên đạn cuối cùng trùng khớp về thời gian. 

Một cách tiếp cận ngây thơ cố gắng liệt kê tất cả các trình tự bắn có thể có hoặc mô phỏng việc bắn mỗi mili giây sẽ không cần thiết và dễ xảy ra lỗi, nhưng quan trọng hơn, nó sẽ gặp khó khăn trong việc diễn giải thời gian hồi chiêu dấu phẩy động một cách chính xác. 

Một cạm bẫy phổ biến khác là bỏ qua mức sát thương trên mỗi viên đạn được giới hạn ở mức$B$, nhưng sát thương thực sự có thể ít hơn trên mỗi viên đạn. Tuy nhiên, vì chúng tôi chỉ kiểm tra xem sát thương có thể đạt được hay không nên chúng tôi giả định trường hợp tốt nhất: mọi viên đạn đều gây ra$B$sát thương nên số lượng đạn tối thiểu cần dùng là:$$\left\lceil \frac{D}{B} \right\rceil$$Vì vậy, vấn đề giảm xuống để kiểm tra xem:$$\left\lceil \frac{D}{B} \right\rceil \le \text{max bullets allowed by firing rate and time}$$với cách xử lý đặc biệt đối với các trường hợp không. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ cố gắng mô phỏng thời gian bắn. Chúng ta có thể lặp lại thời gian theo từng bước nhỏ, lên lịch bắn bất cứ khi nào thời gian hồi chiêu cho phép và tích lũy sát thương. Điều này đúng về mặt khái niệm, nhưng nó gây ra các vấn đề phức tạp và chính xác không cần thiết. Với$S$lên tới 2000 giây và RPM lên tới 2000, mô phỏng có thể cần xem xét hàng nghìn sự kiện kích hoạt tiềm năng cho mỗi trường hợp thử nghiệm và với tối đa$10^3$trường hợp thử nghiệm này trở nên không hiệu quả và dễ vỡ. 

Quan sát quan trọng là chúng ta không cần mô phỏng thời gian. Hạn chế bắn trực tiếp chuyển thành giới hạn đơn giản về số lần bắn trong một khoảng thời gian. Tương tự, giới hạn sát thương giảm xuống giới hạn thấp hơn đối với các lần bắn bắt buộc. Khi cả hai đại lượng được biểu diễn dưới dạng số nguyên, bài toán sẽ trở thành so sánh trực tiếp. 

Vì vậy, thay vì lập mô hình lối chơi, chúng tôi chuyển đổi vật lý thành hai số nguyên: số đạn tối đa có thể và số đạn yêu cầu tối thiểu. Nếu số lượng yêu cầu vượt quá mức tối đa, việc gian lận được đảm bảo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | O(T · S · R) | O(1) | Quá chậm / dễ vỡ | 
| Toán trực tiếp | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc$B, R, D, S$. Chúng xác định sát thương trên mỗi viên đạn, tốc độ bắn, tổng sát thương và khoảng thời gian. 
2. Nếu$R = 0$, vũ khí không thể bắn được. Trong trường hợp này, nếu$D > 0$, xuất ra “gua!” bởi vì bất kỳ thiệt hại là không thể. Nếu không thì xuất ra “ok”. 
3. Nếu$B = 0$, mỗi viên đạn không gây sát thương. Nếu như$D > 0$, không thể thực hiện được dù có bắn hay không, vì vậy hãy xuất ra “gua!”. Nếu như$D = 0$, nó luôn luôn hợp lệ. 
4. Tính số lượng đạn tối thiểu cần thiết để đạt sát thương$D$. Vì mỗi viên đạn đóng góp nhiều nhất$B$, đây là:$$\text{need} = \left\lceil \frac{D}{B} \right\rceil$$Điều này đảm bảo chúng tôi giả định sát thương tối ưu trên mỗi viên đạn. 

1. Tính số lượng đạn tối đa cho phép theo tốc độ bắn trong$S$giây. Mỗi lần bắn yêu cầu$60 / R$giây hồi chiêu sau lần đầu tiên. Sắp xếp lại mang lại:$$\text{max} = \left\lfloor \frac{S \cdot R}{60} \right\rfloor + 1$$Điều này giải thích cho viên đạn đầu tiên ở thời điểm 0. 

1. Nếu$\text{need} > \text{max}$, xuất ra “gua!”, nếu không thì xuất ra “ok”. 

### Tại sao nó hoạt động 

Thuật toán nén tất cả các chuỗi bắn hợp lệ vào một ràng buộc duy nhất: số lần bắn bị giới hạn bởi thời gian và tốc độ, không phụ thuộc vào thời gian chính xác. Bất kỳ trình tự hợp lệ nào đều tương ứng với trình tự thời gian quay không giảm với khoảng cách tối thiểu cố định và các trình tự như vậy được đặc trưng đầy đủ bằng số lượng cảnh quay phù hợp với khoảng thời gian đó. Mặt khác, việc tích lũy sát thương được tối đa hóa bằng cách chỉ định đầy đủ$B$sát thương cho mỗi phát bắn, vì vậy tính khả thi chỉ phụ thuộc vào việc có đủ số phát bắn để tiếp cận hay không$D$. Vì cả hai ràng buộc đều trở thành giới hạn số nguyên chặt chẽ nên việc so sánh chúng là đủ để xác định tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        B, R, D, S = map(int, input().split())

        if D == 0:
            print("ok")
            continue

        if R == 0 or B == 0:
            print("gua!")
            continue

        need = (D + B - 1) // B
        max_shots = (S * R) // 60 + 1

        if need > max_shots:
            print("gua!")
        else:
            print("ok")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo các ràng buộc dẫn xuất một cách trực tiếp. Bộ phận trần`(D + B - 1) // B`tính toán số đạn cần thiết một cách an toàn mà không cần thao tác dấu phẩy động. Công thức số lần bắn tối đa sử dụng số học số nguyên xuyên suốt, tránh các vấn đề về độ chính xác. Trường hợp đặc biệt cho$R = 0$Và$B = 0$được xử lý sớm để tránh số học không hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên:$B = 38, R = 156, D = 152, S = 1$. 

Chúng tôi tính toán số đạn cần thiết như$\lceil 152 / 38 \rceil = 4$. Tốc độ bắn cho phép:$$\left\lfloor \frac{1 \cdot 156}{60} \right\rfloor + 1 = 2 + 1 = 3$$Vì 4 vượt quá 3 nên đầu ra là “gua!”. 

| Bước | Giá trị | 
| --- | --- | 
| D | 152 | 
| B | 38 | 
| cần | 4 | 
| max_shots | 3 | 
| quyết định | gua | 

Điều này cho thấy một hành vi vi phạm cổ điển trong đó sát thương bao hàm nhiều lượt bắn hơn mức có thể thực hiện được trong khoảng thời gian. 

Bây giờ hãy xem xét một trường hợp hợp lệ:$B = 99, R = 51, D = 9, S = 10$. 

Đạn bắt buộc:$$\lceil 9 / 99 \rceil = 1$$Số lần chụp tối đa:$$\left\lfloor \frac{10 \cdot 51}{60} \right\rfloor + 1 = 8 + 1 = 9$$| Bước | Giá trị | 
| --- | --- | 
| D | 9 | 
| B | 99 | 
| cần | 1 | 
| max_shots | 9 | 
| quyết định | được | 

Điều này xác nhận rằng khi thiệt hại nhỏ so với công suất mỗi phát bắn, các ràng buộc sẽ được thỏa mãn một cách tầm thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu các phép tính số học có thời gian không đổi | 
| Không gian | O(1) | Không có dung lượng bổ sung ngoài các biến | 

Các ràng buộc cho phép lên đến$10^3$trường hợp thử nghiệm, do đó quét tuyến tính dễ dàng đủ nhanh. Tất cả các phép toán đều là số học số nguyên, làm cho giải pháp vừa hiệu quả vừa an toàn về mặt số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        B, R, D, S = map(int, input().split())

        if D == 0:
            out.append("ok")
            continue

        if R == 0 or B == 0:
            out.append("gua!")
            continue

        need = (D + B - 1) // B
        max_shots = (S * R) // 60 + 1

        out.append("gua!" if need > max_shots else "ok")

    return "\n".join(out)

# provided samples
assert run("1\n38 156 152 1\n") == "gua!"
assert run("1\n280 25 280 0\n") == "ok"

# custom cases
assert run("1\n0 0 1 1\n") == "gua!"          # no damage possible
assert run("1\n10 60 0 5\n") == "ok"          # zero damage always ok
assert run("1\n5 60 100 1\n") == "gua!"       # impossible damage burst
assert run("1\n10 60 10 0\n") == "ok"         # single instant shot
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| R = 0 với D > 0 | gua! | không thể bắn | 
| D = 0 | được | tính khả thi tầm thường | 
| sát thương cao, S nhỏ | gua! | vi phạm giới hạn tỷ lệ | 
| ranh giới S = 0 | được/gua! sự đúng đắn | hành vi cửa sổ một lần chụp | 

## Vỏ cạnh 

Khi nào$R = 0$, thuật toán ngay lập tức phân loại mọi thiệt hại tích cực là không thể xảy ra. Điều này tránh việc chia cho 0 hoặc giả định không chính xác một số khả năng bắn tiềm ẩn. Ví dụ, đầu vào$B=99, R=0, D=1, S=10$dẫn trực tiếp đến “gua!”, vì không thể bắn được viên đạn nào. 

Khi$S = 0$, công thức vẫn hoạt động chính xác vì$(S \cdot R) // 60 + 1 = 1$, nghĩa là chỉ có thể tồn tại một viên đạn trong dòng thời gian. Nếu như$D$đòi hỏi nhiều hơn một dấu đầu dòng, sự bất bình đẳng đương nhiên sẽ thất bại. 

Khi$B = 0$, thiệt hại cần thiết trở nên không thể đạt được trừ khi$D = 0$, vì không có số lượng hữu hạn viên đạn có sức sát thương bằng 0 có thể bắn trúng mục tiêu. Việc kiểm tra sớm sẽ ngăn chặn việc phân chia trần không hợp lệ. 

Những trường hợp này cho thấy rằng tất cả các đầu vào bệnh lý đều được chuyển thành các phép so sánh số nguyên mà không cần logic mô phỏng đặc biệt.
