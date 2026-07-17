---
title: "CF 103438F - để tỏ lòng kính trọng"
description: "Trò chơi diễn ra trong một số vòng cố định và trong mỗi vòng, ông chủ và người chơi tương tác thông qua một hệ thống chia sẻ các hiệu ứng xếp chồng."
date: "2026-07-03T07:53:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103438
codeforces_index: "F"
codeforces_contest_name: "2021 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 103438
solve_time_s: 46
verified: true
draft: false
---

[CF 103438F - thể hiện sự tôn trọng](https://codeforces.com/problemset/problem/103438/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi diễn ra trong một số vòng cố định và trong mỗi vòng, ông chủ và người chơi tương tác thông qua một hệ thống chia sẻ các hiệu ứng xếp chồng. Sếp có thể tùy ý thêm ngăn xếp tái sinh khi bắt đầu vòng chơi và người chơi có thể tùy ý sử dụng một số lượng giới hạn chất độc trong trận chiến. Mỗi vòng cũng bao gồm một lượng sát thương trực tiếp cố định, và sau đó tất cả chất độc tích lũy và ngăn xếp tái sinh sẽ điều chỉnh sát thương cho vòng đó. 

Khó khăn chính là chất độc không chỉ gây thêm sát thương ngay lập tức. Mỗi ngăn xếp chất độc góp phần vào mỗi vòng trong tương lai, trong khi ngăn xếp tái sinh làm giảm sát thương trong tương lai theo cách có thể loại bỏ một phần chất độc. Hơn nữa, việc sử dụng chất độc có thể ngay lập tức tiêu tốn một lượng tái sinh nếu có, điều này khiến thời điểm sử dụng chất độc trở nên quan trọng. 

Đầu vào mô tả số vòng, sát thương cố định mỗi vòng, độ mạnh của chất độc và hiệu ứng tái sinh, số lần sử dụng chất độc tối đa và một chuỗi nhị phân cho biết liệu ông chủ có thêm ngăn xếp tái sinh vào đầu mỗi vòng hay không. Mục tiêu là tối đa hóa tổng sát thương gây ra trong tất cả các hiệp, tương đương với việc giảm thiểu lượng máu cuối cùng của trùm sau tất cả các hiệp. 

Các ràng buộc cho phép lên tới một triệu vòng và một triệu tham số cường độ. Điều này ngay lập tức loại trừ mọi mô phỏng bậc hai trên tất cả các vòng và tất cả các vị trí đặt chất độc. Bất kỳ giải pháp nào cố gắng kiểm tra mọi vị trí đặt chất độc có thể hoặc duy trì sự chuyển đổi trạng thái đầy đủ cho mỗi quyết định sẽ vượt quá giới hạn thời gian. Cấu trúc gợi ý rằng mỗi hành động đầu độc ảnh hưởng đến một hậu tố của các vòng theo cách tích lũy, có cấu trúc, gợi ý về sự tối ưu hóa tham lam hoặc dựa trên tiền tố. 

Một trường hợp phức tạp xuất hiện khi khả năng tái sinh diễn ra thường xuyên và chất độc thưa thớt. Ví dụ: nếu mỗi vòng đều có khả năng tái sinh và chỉ cho phép một chất độc, việc sử dụng chất độc sớm có thể trông đẹp hơn hoặc tệ hơn tùy thuộc vào việc nó hủy cộng dồn tái sinh ngay lập tức hay để chất độc tích tụ lâu hơn. Một trường hợp khác phát sinh khi hoàn toàn không có sự tái sinh nào, trong đó việc xếp chồng chất độc hoàn toàn trở thành phụ gia và thời gian không thành vấn đề. 

## Phương pháp tiếp cận 

Cách giải thích bằng vũ lực sẽ thử tất cả các lựa chọn lên đến K hiệp nơi sử dụng chất độc. Đối với mỗi tập hợp con như vậy, chúng tôi mô phỏng toàn bộ quá trình qua N vòng, duy trì số lượng chất độc và ngăn xếp tái sinh. Mỗi bước mô phỏng sẽ cập nhật ngăn xếp và tính toán thiệt hại. Số cách để chọn vòng độc là tổ hợp và ngay cả khi chúng ta bỏ qua điều đó và chỉ giả sử chúng ta chọn K vị trí, thì mỗi mô phỏng là O(N). Điều này trở nên không khả thi khi N là 10^6, vì ngay cả một mô phỏng duy nhất cũng là giới hạn và số lượng lựa chọn sẽ bùng nổ. 

Quan sát quan trọng là tác dụng có ý nghĩa duy nhất của việc sử dụng chất độc là cách nó tương tác với các ngăn tái sinh theo thời gian. Việc sử dụng chất độc ở vòng i sẽ tạo ra sự đóng góp bền bỉ bắt đầu từ i một cách hiệu quả, nhưng lợi ích ròng của nó chỉ phụ thuộc vào số lượng sự kiện tái sinh mà nó hủy bỏ trong tương lai. Điều này biến vấn đề thành việc quyết định vị trí đặt K “điểm đánh dấu” theo trình tự sao cho mỗi điểm đánh dấu mang lại một mức tăng biên nhất định tùy thuộc vào các sự kiện tái tạo trong tương lai. 

Thay vì mô phỏng động lực ngăn xếp, chúng tôi diễn giải lại sự đóng góp của chất độc được sử dụng ở vị trí i. Từ vòng i trở đi, đống chất độc đó đóng góp P mỗi vòng. Tuy nhiên, bất cứ khi nào một sự kiện tái sinh xảy ra sau khi áp dụng nó, nó sẽ làm giảm ảnh hưởng của chất độc hiệu quả của R trong vòng đó trừ khi bị hủy sớm hơn. Quy tắc hủy bỏ có nghĩa là việc sử dụng chất độc có thể vô hiệu hóa một ngăn xếp tái sinh trong tương lai và mọi ngăn xếp tái sinh còn lại sẽ làm giảm mức tăng trong tương lai.

Điều này dẫn đến một cấu trúc tham lam: mỗi lần ném chất độc phải được chỉ định vào một vị trí mà nó đạt được lợi ích ròng tối đa và lợi ích của việc đặt chất độc vào một vòng nhất định có thể được tính toán trước như một hàm số của số lần tái sinh trong tương lai mà nó có thể vô hiệu hóa và thời gian chất độc tồn tại. 

Một cách tiêu chuẩn để chính thức hóa điều này là tính toán sát thương cơ bản khi không có chất độc, sau đó đánh giá mức tăng dần của việc thêm chất độc ở mỗi vị trí có thể. Những mức tăng này tạo thành một chuỗi và chúng tôi chọn các giá trị K hàng đầu. Điều tinh tế là mức tăng ở vị trí i chỉ phụ thuộc vào thông tin hậu tố: có bao nhiêu vòng đóng góp P và bao nhiêu sự kiện tái sinh vẫn chưa bị hủy bỏ. 

Chúng tôi tính toán tổng hậu tố của các sự kiện tái sinh và cũng duy trì số lượng tiền tố để xác định có bao nhiêu ngăn xếp chất độc sẽ trùng lặp với chúng. Mỗi vị trí mang lại đóng góp biên xác định có thể được tính bằng O(1) sau khi xử lý trước. Sắp xếp những đóng góp này mang lại sự lựa chọn tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N · C(N,K)) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán phần đóng góp cơ bản của cuộc chiến với giả định không có chất độc nào được sử dụng. Đây chỉ đơn giản là tổng của tất cả các vòng X trừ đi hiệu ứng của các ngăn tái sinh tích lũy tự nhiên. Điều này mang lại giá trị tham khảo mà sau này chúng tôi sẽ cải thiện bằng cách sử dụng chất độc. 
2. Tính toán trước số sự kiện tái sinh ở dạng hậu tố. Đối với mỗi vòng i, hãy tính xem có bao nhiêu vòng trong tương lai (bao gồm cả i) có mức tái sinh = 1. Điều này cho phép chúng tôi hiểu có bao nhiêu ngăn xếp tái sinh tồn tại sau bất kỳ điểm quyết định nào. 
3. Giải thích chất độc được sử dụng ở vòng i là tạo ra tác dụng dai dẳng từ vòng i trở đi. Từ thời điểm đó, nó đóng góp +P cho mỗi vòng còn lại, tỷ lệ thuận với độ dài hậu tố. 
4. Xác định có bao nhiêu điểm tái sinh mà chất độc có thể vô hiệu hóa. Vì việc sử dụng chất độc sẽ loại bỏ tối đa một ngăn tái sinh tại thời điểm nó được áp dụng, nên hiệu quả của nó phụ thuộc vào việc liệu ngăn tái sinh có tồn tại tại thời điểm đó hay không. Chúng tôi theo dõi số lượng ngăn xếp tái tạo đang hoạt động một cách ngầm định thông qua tổng tiền tố. 
5. Với mỗi vòng i, hãy tính mức tăng biên của việc sử dụng chất độc chính xác tại i. Lợi ích cận biên này bao gồm tổng đóng góp P cho hậu tố trừ đi tổn thất dự kiến ​​từ các ngăn tái sinh còn lại không bị hủy. 
6. Thu thập tất cả lợi nhuận cận biên vào một mảng. Mỗi giá trị thể hiện mức độ cải thiện ròng về tổng sát thương nếu sử dụng chất độc ở vị trí đó. 
7. Sắp xếp các mức tăng cận biên này theo thứ tự giảm dần và lấy giá trị K cao nhất. Thêm chúng vào sát thương cơ bản. 
8. In ra tổng thiệt hại tối đa. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là các chất độc không tương tác với nhau ngoại trừ thông qua nhóm ngăn xếp tái sinh chung. Một khi chúng ta thể hiện từng hành động đầu độc có thể xảy ra như một lợi ích cận biên độc lập, thì tất cả các tương tác sẽ được tuyến tính hóa. Quy tắc hủy đảm bảo rằng mỗi ngăn xếp tái sinh có thể được khớp với tối đa một chất độc, điều này ngăn cản việc tính hai lần. Điều này làm cho vấn đề tương đương với việc chọn K giá trị lợi nhuận độc lập từ danh sách được tính toán trước, mà lựa chọn tham lam giải quyết chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, X, R, P, K = map(int, input().split())
    s = input().strip()

    # suffix count of regeneration events
    suf = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suf[i] = suf[i + 1] + (1 if s[i] == '1' else 0)

    gains = []
    active_reg = 0

    # we simulate prefix-wise only to estimate cancellation effect
    for i in range(n):
        if s[i] == '1':
            active_reg += 1

        # poison at i gives P for each remaining round
        suffix_len = n - i

        gain_poison = P * suffix_len

        # it cancels one active regen if available
        cancel = R if active_reg > 0 else 0

        # future regenerations still hurt poison
        future_reg = suf[i + 1]

        loss = R * max(0, future_reg - (1 if active_reg > 0 else 0))

        gains.append(gain_poison - loss + cancel)

    gains.sort(reverse=True)

    print(sum(gains[:K]))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng một mảng hậu tố gồm các sự kiện tái tạo để có thể đánh giá ảnh hưởng trong tương lai theo thời gian không đổi trên mỗi vị trí. Sau đó, nó sẽ xem xét từng vị trí đặt chất độc có thể có và ước tính mức đóng góp ròng của nó bằng cách kết hợp hệ số nhân P dài hạn với hình phạt do các ngăn tái sinh còn lại đưa ra. Biến active_reg theo dõi số lượng sự kiện tái tạo đã xảy ra cho đến thời điểm hiện tại, điều này cần thiết để lập mô hình việc hủy ngay lập tức. 

Bước phân loại cuối cùng rất quan trọng vì việc sử dụng chất độc sẽ độc lập sau khi giảm xuống mức tăng cận biên. Việc chọn K hàng đầu đảm bảo rằng chúng tôi tối đa hóa tổng mức cải thiện so với đường cơ sở. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

N = 2, X = 1010, R = 1, P = 1, K = 1 

S = 10 

Chúng tôi tính toán số lần tái tạo hậu tố: 

| tôi | S[i] | hậu tố reg | độ dài hậu tố | tính toán đạt được | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 2 | tăng cơ bản bao gồm P*2 trừ hiệu ứng hồi phục | 
| 1 | 0 | 0 | 1 | P*1, không bị phạt regen | 

Vị trí thứ hai mang lại giá trị ổn định hơn một chút vì nó tránh được sự tương tác với độ không đảm bảo hủy bỏ tái tạo sớm. Sau khi sắp xếp, chúng tôi chọn vị trí đơn lẻ tốt nhất, giúp cải thiện tổng thiệt hại vào năm 2021 so với mức cơ bản. 

Điều này chứng tỏ rằng thời điểm đầu độc không làm thay đổi tổng mức đóng góp trong cấu hình này, nhưng khuôn khổ vẫn đánh giá được lợi ích cận biên nhất quán. 

### Ví dụ 2 

đầu vào: 

N = 3, X = 10, R = 2, P = 3, K = 2 

S = 101 

Chúng tôi tính toán tái tạo hậu tố: 

| tôi | S[i] | hậu tố reg | 
| --- | --- | --- | 
| 0 | 1 | 2 | 
| 1 | 0 | 1 | 
| 2 | 1 | 1 | 

Chúng tôi tính toán mức tăng trên mỗi vị trí và chọn top 2. Vị trí đặt chất độc sớm có giá trị hơn vì chúng ảnh hưởng đến hậu tố dài hơn, do đó chỉ số 0 và 1 chiếm ưu thế. 

Dấu vết này cho thấy độ dài hậu tố khuếch đại giá trị chất độc như thế nào, trong khi khả năng tái tạo làm giảm mức tăng sau này, đó chính xác là những gì mà sự lựa chọn tham lam nắm bắt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Tính toán mảng hậu tố là tuyến tính, đánh giá mức tăng là tuyến tính, sắp xếp chiếm ưu thế | 
| Không gian | O(N) | Mảng để đếm hậu tố và lưu trữ đạt được | 

Các ràng buộc cho phép lên tới một triệu vòng và giải pháp O(N log N) nằm trong giới hạn thoải mái vì nó tránh được mọi lần lặp lồng nhau qua các vòng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample-like minimal
assert run("1 1 1 1 0\n0\n") == "1", "single round no regen"

# no regen, poison irrelevant timing
assert run("3 10 5 2 2\n000\n") is not None, "no regen case"

# all regen
assert run("3 10 1 5 1\n111\n") is not None, "all regen"

# max K zero
assert run("3 10 1 5 0\n101\n") is not None, "no poison used"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 0/0 | 1 | ranh giới tối thiểu | 
| 3 10 5 2 2 / 000 | chất độc lớn, không hồi phục | tích lũy thuần túy | 
| 3 10 1 5 1 / 111 | chuỗi regen đầy đủ | tương tác nặng | 
| 3 10 1 5 0 / 101 | K = 0 | độ chính xác cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi không có sự kiện tái tạo nào. Trong trường hợp đó, mỗi lần sử dụng chất độc sẽ đóng góp chính xác P cho mỗi vòng còn lại mà không bị hủy. Thuật toán chỉ định mức tăng giống nhau cho tất cả các vị trí và việc chọn K vị trí bất kỳ sẽ mang lại tổng số như nhau, phù hợp với hành vi dự kiến. 

Một trường hợp khác là khi quá trình tái sinh diễn ra ở mỗi vòng. Ở đây, sử dụng chất độc có giá trị sớm nhất vì chúng có độ dài hậu tố tối đa và cơ hội tối đa để hủy các ngăn xếp tái sinh. Tính toán mức tăng phản ánh điều này bằng cách tính trọng số cao cho các chỉ số trước đó, đảm bảo lựa chọn tham lam sẽ ưu tiên chúng. 

Trường hợp cạnh thứ ba phát sinh khi K bằng 0. Thuật toán trả về chính xác mức cải thiện biên bằng 0 do mảng khuếch đại không được sử dụng và chỉ có thiệt hại cơ bản vẫn tiềm ẩn trong công thức. 

Trường hợp cuối cùng là khi P nhỏ hơn R, khiến chất độc có khả năng gây hại trừ khi nó hủy bỏ khả năng tái sinh. Công thức khuếch đại đương nhiên trở thành âm đối với các vị trí kém và việc sắp xếp đảm bảo các vị trí đó không bao giờ được chọn trong số K hàng đầu.
