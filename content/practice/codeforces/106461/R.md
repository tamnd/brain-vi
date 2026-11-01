---
title: "CF 106461R - Kéo giấy Rock"
description: "Chúng ta được cấp hai số nguyên mô tả thông tin tổng hợp về một nhóm người chơi trò chơi Kéo-Giấy-Kéo đơn giản hóa. Thay vì theo dõi các tương tác riêng lẻ, chúng tôi chỉ quan tâm đến việc có bao nhiêu người chọn mỗi tay: oẳn tù tì và giấy."
date: "2026-06-19T15:31:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106461
codeforces_index: "R"
codeforces_contest_name: "KUPC 2025 (The 4th Universal Cup. Stage 22: GP of Kyoto)"
rating: 0
weight: 106461
solve_time_s: 42
verified: true
draft: false
---

[CF 106461R - Kéo giấy Rock](https://codeforces.com/problemset/problem/106461/R) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp hai số nguyên mô tả thông tin tổng hợp về một nhóm người chơi trò chơi Kéo-Giấy-Kéo đơn giản hóa. Thay vì theo dõi các tương tác riêng lẻ, chúng tôi chỉ quan tâm đến việc có bao nhiêu người chọn mỗi tay: oẳn tù tì và giấy. Hãy để những số đó được tính$R$,$S$, Và$P$. Họ phải tính tổng bằng tổng số người chơi$N$, và họ cũng phải thỏa mãn ràng buộc tuyến tính thứ hai liên quan đến kéo và giấy: gấp đôi số kéo cộng với năm lần số người chơi giấy bằng một giá trị nhất định$M$. 

Nhiệm vụ không phải là xây dựng một cấu hình thực tế mà chỉ để xác định xem có bất kỳ bộ ba số nguyên không âm nào hay không$(R, S, P)$tồn tại thỏa mãn cả hai phương trình. Ngoài ra, chúng tôi cũng cần xác định xem có tồn tại cấu hình hợp lệ trong đó trò chơi có người chiến thắng hay không. Tuyên bố mã hóa điều kiện này như một thuộc tính cấu trúc: người chiến thắng tồn tại chính xác khi không phải cả ba loại bài đều xuất hiện đồng thời, nghĩa là nhiều nhất là hai trong số đó.$R, S, P$là tích cực. 

Mặc dù vấn đề được thể hiện dưới dạng một trò chơi, cấu trúc cốt lõi hoàn toàn là tính khả thi số học trên một hệ thống nhỏ các ràng buộc tuyến tính với các biến số nguyên không âm. 

Các ràng buộc hàm ý một không gian tìm kiếm rất nhỏ xét theo bậc tự do. Một lần$S$Và$P$được chọn,$R$bị ép buộc, do đó hệ thống thực sự chuyển sang kiểm tra nghiệm số nguyên trên mạng một chiều hoặc hai chiều tùy thuộc vào cách chúng ta loại bỏ các biến. Điều này gợi ý mạnh mẽ rằng việc ép buộc cả hai biến lên tới$N$sẽ quá chậm đối với kích thước lớn$N$, vì đó sẽ là bậc hai trong trường hợp xấu nhất. 

Điểm tinh tế chính là tính khả thi phụ thuộc vào tính nhất quán của số nguyên, không chỉ các giải pháp có giá trị thực. Một sai lầm phổ biến là giải hệ thống tuyến tính trên số thực và giả định tính khả thi vẫn tiếp tục, nhưng các ràng buộc về tính chia hết có thể loại bỏ hầu hết các ứng cử viên. 

Các trường hợp khó khăn phát sinh khi các giải pháp nằm trên ranh giới nơi một hoặc nhiều$R, S, P$trở thành số không. Ví dụ, nếu$P = 0$, phương trình giảm xuống còn$2S = M$, có thể chẵn hoặc không. Tương tự, nếu$S = 0$, chúng tôi nhận được$5P = M$, buộc phải chia hết cho 5. Các trường hợp biên này quan trọng cả về tính khả thi và điều kiện "người chiến thắng tồn tại". 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các cặp$(S, P)$như vậy$2S + 5P = M$, tính toán$R = N - S - P$và kiểm tra xem tất cả các biến có phải là không âm hay không. Bản thân phương trình đã hạn chế đáng kể không gian tìm kiếm:$P$phạm vi lên đến$M/5$và với mỗi lựa chọn, chúng tôi tính toán$S = (M - 5P)/2$nếu nó là số nguyên. Điều này đã là tuyến tính trong$M$, có thể chấp nhận được đối với các ràng buộc nhỏ nhưng trở nên không hiệu quả khi$M$lớn, đặc biệt nếu có nhiều trường hợp thử nghiệm tồn tại. 

Quan sát quan trọng là hệ thống này không thực sự hai chiều. Phương trình thứ hai xác định mối quan hệ giữa$S$Và$P$, thế nên một lần$P$được chọn,$S$được xác định duy nhất nếu nó hợp lệ. Sau đó$R$bị ép buộc. Vì vậy, vấn đề trở thành kiểm tra xem có tồn tại số nguyên không âm hay không$P$sao cho cả hai ràng buộc về tính chia hết và phạm vi đều được thỏa mãn. 

Phần thứ hai, về việc liệu người chiến thắng có tồn tại hay không, có cấu trúc đơn giản hơn. Người chiến thắng tồn tại chính xác khi không phải cả ba điều trên$R, S, P$là tích cực. Vì vậy, khi chúng tôi tìm thấy bất kỳ giải pháp hợp lệ nào, chúng tôi cũng kiểm tra xem liệu ít nhất một trong các biến có bằng 0 hay không. Nếu một giải pháp tồn tại trong đó tất cả đều dương, thì điều đó tương ứng với trạng thái hỗn hợp hoàn toàn và do đó không có người chiến thắng trong điều kiện đã cho. Nếu tất cả các giải pháp hợp lệ nhất thiết buộc phải có thành phần bằng 0 thì có thể có người chiến thắng. 

Điều này làm giảm vấn đề khi quét một phạm vi nhỏ$P$, tính toán tương ứng$S$, suy ra$R$và kiểm tra các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu (S, P) | O(M) | O(1) | Quá chậm đối với M lớn | 
| Chỉ lặp lại P | O(M) trường hợp xấu nhất, O(M/5) hiệu quả | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các giá trị có thể có của$P$từ 0 đến$\lfloor M/5 \rfloor$. Điều này là đủ vì mỗi tờ giấy đóng góp 5 đơn vị cho phương trình thứ hai và bất kỳ giá trị nào lớn hơn sẽ ngay lập tức vượt quá$M$. 
2. Đối với mỗi$P$, tính phần đóng góp còn lại cho cái kéo như$M - 5P$. Đại lượng này phải là số chẵn, nếu không thì không có số nguyên$S$tồn tại. 
3. Nếu chẵn thì tính$S = (M - 5P)/2$. Bước này thực thi sự thỏa mãn chính xác của phương trình thứ hai mà không cần số học dấu phẩy động, điều này tránh được các vấn đề về độ chính xác. 
4. Tính toán$R = N - S - P$. Điều này xuất phát trực tiếp từ hạn chế về tổng dân số. 
5. Kiểm tra xem$R, S, P$đều không âm. Nếu bất kỳ kết quả nào là âm, hãy loại bỏ ứng cử viên này vì nó vi phạm tính khả thi trong việc diễn giải số đếm. 
6. Nếu tìm thấy bộ ba hợp lệ, hãy ghi lại tính khả thi. Ngoài ra, hãy theo dõi xem có tồn tại ít nhất một bộ ba hợp lệ trong đó ít nhất một biến bằng 0 hay không, vì điều đó xác định liệu cấu hình chiến thắng có tồn tại hay không. 

Tại sao nó hoạt động dựa trên thực tế là phương trình thứ hai xác định mạng một chiều gồm các điểm nguyên trong$(S, P)$máy bay. Mọi giải pháp hợp lệ đều phải nằm trên mạng này và mọi ứng cử viên đều được xác định đầy đủ bằng cách chọn$P$. Phương trình đầu tiên sau đó hoạt động như một bộ lọc chỉ chọn những điểm mạng chiếu chính xác lên$N$. Bởi vì cả hai ràng buộc đều là tuyến tính và chúng tôi thực thi tính tích phân ở mỗi bước nên không có giải pháp hợp lệ nào bị bỏ qua và không có giải pháp không hợp lệ nào được chấp nhận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())

    possible = False
    has_winner = False

    for P in range(M // 5 + 1):
        rem = M - 5 * P
        if rem < 0:
            break
        if rem % 2 != 0:
            continue

        S = rem // 2
        R = N - S - P

        if R < 0:
            continue

        possible = True
        if R == 0 or S == 0 or P == 0:
            has_winner = True

    if not possible:
        print("NO")
    else:
        print("YES" if has_winner else "NO")

if __name__ == "__main__":
    solve()
```Mã trực tiếp theo sau bảng liệt kê$P$. Việc kiểm tra tính chẵn lẻ đảm bảo rằng$S$vẫn là số nguyên, tránh các nửa giá trị không hợp lệ. Một lần$S$có nguồn gốc,$R$bị ép buộc bởi phương trình đầu tiên. 

Một chi tiết triển khai tinh tế là sự nghỉ ngơi sớm khi$M - 5P < 0$, điều này không cần thiết về mặt kỹ thuật nhưng ngăn chặn việc lặp lại thêm trong các đầu vào bệnh lý. Một chi tiết quan trọng khác là kiểm tra tính không âm sau khi tính toán$R$, vì ngay cả khi phương trình thứ hai được thỏa mãn, ràng buộc thứ nhất vẫn có thể làm mất hiệu lực của ứng viên. 

Điều kiện để giành chiến thắng được theo dõi trong quá trình liệt kê, vì nó chỉ phụ thuộc vào việc liệu có bất kỳ giải pháp hợp lệ nào đặt ít nhất một biến ở mức 0 hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$N = 6, M = 10$. 

| P | rem = M-5P | S | R = N-S-P | hợp lệ | Không có mặt | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 10 | 5 | 1 | vâng | vâng | 

Chúng tôi tìm thấy một cấu hình hợp lệ$(R, S, P) = (1, 5, 0)$. Điều này thỏa mãn cả hai phương trình. Sự hiện diện của$P = 0$chỉ ra rằng không phải cả ba tay đều có mặt, do đó tồn tại cấu hình người chiến thắng. 

### Ví dụ 2 

Hãy xem xét$N = 8, M = 13$. 

| P | rem | S | R | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 13 | - | - | không | 
| 1 | 8 | 4 | 3 | vâng | 

Chúng tôi nhận được một bộ ba hợp lệ$(3, 4, 1)$, nhưng tất cả các giá trị đều dương. Điều này tương ứng với trạng thái trò chơi hỗn hợp hoàn toàn, vì vậy không có cấu hình người chiến thắng nào tồn tại trong điều kiện này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M/5)$| Chúng tôi lặp lại tất cả các giá trị khả thi của$P$, từng được kiểm tra trong O(1) | 
| Không gian |$O(1)$| Chỉ có một số lượng biến không đổi được duy trì | 

Phép lặp là tuyến tính theo số lượng giá trị khả thi của$P$, được giới hạn bởi$M$. Đối với các ràng buộc điển hình trong các bài toán kiểu Codeforces, điều này đủ hiệu quả khi kết hợp với việc cắt tỉa sớm và kiểm tra liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()
    return out.getvalue().strip()

# minimal valid case
assert run("1 0\n") == "YES", "trivial all zero"

# simple valid configuration
assert run("6 10\n") == "YES", "example valid with zero component"

# valid but no winner
assert run("8 13\n") == "NO", "all positive solution only"

# impossible case
assert run("5 1\n") == "NO", "no integer solution"

# boundary case large P only
assert run("10 50\n") == "YES", "multiple solutions possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | CÓ | tính khả thi tối thiểu | 
| 6 10 | CÓ | giải pháp không có thành phần | 
| 8 13 | KHÔNG | giải pháp hoàn toàn tích cực | 
| 5 1 | KHÔNG | không có điểm mạng số nguyên | 
| 10 50 | CÓ | nhiều ứng viên hợp lệ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$M$thật kỳ quặc. Trong trường hợp này, mọi ứng viên$P$tạo ra số dư lẻ$M - 5P$bất cứ khi nào$5P$chẵn hoặc lẻ không nhất quán, nhưng trong thực tế, tính chẵn lẻ ngay lập tức loại bỏ tất cả các ứng cử viên trong đó$S$sẽ không phải là số nguyên. Ví dụ,$N = 10, M = 3$không mang lại giải pháp hợp lệ vì$2S = 3 - 5P$không thể chẵn với bất kỳ số nguyên nào$P$. 

Một trường hợp cạnh khác là khi$P = 0$, làm giảm hệ thống xuống$2S = M$Và$R = N - S$. Ví dụ,$N = 7, M = 4$cho$S = 2, R = 5$, một giải pháp hợp lệ trong đó một biến bằng 0, đảm bảo cấu hình chiến thắng. 

Trường hợp cạnh thứ ba là khi tất cả các biến trở nên dương hoàn toàn nhưng vẫn thỏa mãn cả hai phương trình. Ví dụ,$N = 8, M = 13$sản xuất$(R, S, P) = (3, 4, 1)$. Thuật toán đánh dấu chính xác tính khả thi nhưng không đánh dấu người chiến thắng vì không có biến nào bằng 0 trong không gian giải pháp hợp lệ duy nhất.
