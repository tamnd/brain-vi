---
title: "CF 104785L - Người đứng cuối cùng"
description: "Hai người chơi mỗi người điều khiển một đơn vị chiến đấu duy nhất. Mỗi đơn vị bắn liên tục tên lửa vào một khoảng thời gian cố định. Một phát bắn không gây sát thương ngay lập tức, nó sẽ hạ cánh nửa giây sau khi được bắn. Khi một đơn vị đã bắn, nó phải đợi thời gian nạp lại trước khi có thể bắn lại."
date: "2026-06-28T14:42:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104785
codeforces_index: "L"
codeforces_contest_name: "2023 United Kingdom and Ireland Programming Contest (UKIEPC 2023)"
rating: 0
weight: 104785
solve_time_s: 58
verified: true
draft: false
---

[CF 104785L - Người đứng cuối cùng](https://codeforces.com/problemset/problem/104785/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai người chơi mỗi người điều khiển một đơn vị chiến đấu duy nhất. Mỗi đơn vị bắn liên tục tên lửa vào một khoảng thời gian cố định. Một phát bắn không gây sát thương ngay lập tức, nó sẽ hạ cánh nửa giây sau khi được bắn. Khi một đơn vị đã bắn, nó phải đợi thời gian nạp lại trước khi có thể bắn lại. Cả hai đơn vị đều bắt đầu hoàn toàn sẵn sàng vào thời điểm 0 và ngay lập tức bắn tên lửa đầu tiên. 

Mỗi tên lửa làm giảm lượng máu của đối thủ một lượng cố định khi nó tiếp đất. Một đơn vị sẽ bị tiêu diệt ngay khi máu của nó trở thành 0 hoặc âm. Bởi vì cả hai bên có thể bắn cùng một lúc và tên lửa của họ có độ trễ di chuyển như nhau, nên cả hai đơn vị có thể chết “ngay lập tức” nếu phát bắn sát thương cuối cùng của họ hạ cánh đồng thời. 

Nhiệm vụ là xác định đơn vị nào vẫn còn sức khỏe tích cực sau khi đơn vị kia bị tiêu diệt, giả sử cả hai đều hành xử theo cách tối ưu duy nhất hiện có, đó chỉ đơn giản là bắn bất cứ khi nào chúng được phép bắn. 

Đầu vào bao gồm hai bộ ba. Bộ ba đầu tiên mô tả đơn vị của người chơi về lượng máu, sát thương mỗi phát bắn và thời gian nạp lại. Bộ ba thứ hai mô tả đơn vị của người chơi thứ hai theo cùng một định dạng. Kết quả là người chơi thứ nhất thắng, người chơi thứ hai thắng hoặc không ai có thể vượt lên dẫn trước về thời gian sống sót. 

Các ràng buộc rất nhỏ, với tất cả các giá trị lên tới 1000. Điều này loại trừ mọi nhu cầu về cấu trúc dữ liệu nặng hoặc tối ưu hóa nâng cao, nhưng nó vẫn còn chỗ cho nhiều cảnh quay theo thời gian nếu được mô phỏng một cách ngây thơ. 

Mô phỏng trực tiếp theo các bước thời gian nhỏ sẽ gây hiểu lầm vì các sự kiện xảy ra thưa thớt ở những khoảng thời gian không đều. Một sai lầm phổ biến khác là bỏ qua độ trễ di chuyển 0,5 giây, điều này có thể gây ra thứ tự không chính xác khi cả hai đơn vị tung ra đòn kết liễu trong cùng một chu kỳ. 

Một trường hợp phức tạp xuất hiện khi cả hai đơn vị tiêu diệt lẫn nhau trên cùng một chỉ số trúng đích. Ví dụ: nếu cả hai đều không còn máu sau khi tên lửa thứ ba hạ cánh, cả hai đều chết cùng lúc ngay cả khi một người bắn sớm hơn trong thời gian thực, bởi vì cả hai sự kiện sát thương đều hạ cánh đồng thời. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng cuộc chiến trong thời gian liên tục. Mỗi đơn vị duy trì một bộ đếm thời gian cho lần bắn tiếp theo và chúng tôi nâng cao thời gian cho sự kiện bắn tiếp theo, lập lịch trình gây sát thương vào thời điểm cộng thêm 0,5. Chúng tôi tiếp tục áp dụng các sự kiện gây sát thương theo thứ tự thời gian cho đến khi một hoặc cả hai giá trị sức khỏe giảm xuống 0. 

Mô phỏng này đúng, nhưng số lượng sự kiện có thể lớn. Mỗi đơn vị có thể bắn tối đa khoảng 1000 lần, vì máu và sát thương bị giới hạn là 1000 và sát thương mỗi phát bắn có thể nhỏ bằng 1. Mô phỏng đầy đủ theo sự kiện vẫn nằm trong giới hạn, nhưng việc di chuyển theo thời gian hoặc duy trì dòng thời gian chi tiết là chi phí không cần thiết. 

Quan sát quan trọng là thứ tự chính xác bên trong độ trễ di chuyển 0,5 giây không thành vấn đề. Mọi phát bắn của cả hai bên đều có độ trễ như nhau, do đó thứ tự tương đối chỉ phụ thuộc vào số lượng phát bắn cần thiết để tiêu diệt một đơn vị và tần suất những phát bắn đó xảy ra. 

Thay vì mô phỏng từng tên lửa, chúng tôi tính toán cần bao nhiêu lần bắn trúng để tiêu diệt từng đơn vị. Nếu người chơi có máu h1 và nhận sát thương d2 cho mỗi lần đánh, thì số lần đánh cần thiết là ceil(h1 / d2). Những lần truy cập đó xảy ra ở những khoảng thời gian cố định là t2 giây và lần truy cập thứ k đến vào thời điểm (k−1)·t2 + 0,5. Điều tương tự cũng áp dụng đối xứng cho người chơi thứ hai. 

So sánh hai thời điểm tử vong trực tiếp cho chúng ta biết người chiến thắng. Độ trễ +0,5 được chia sẻ sẽ bị hủy khi so sánh, do đó chỉ có lịch trình bắn là quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Sự kiện O(H1/d2 + H2/d1) (hoặc tệ hơn với bước thời gian ngây thơ) | O(nhật ký 1 sự kiện) | Được chấp nhận nhưng không cần thiết | 
| Tính toán tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính xem một người chơi cần bao nhiêu đòn để chết. Đây là số nguyên nhỏ nhất k1 sao cho k1·d2 ≥ h1. Điều này thể hiện số lượng tác động thành công mà người chơi hai phải đạt được. 
2. Tính xem hai người chơi cần bao nhiêu đòn để chết theo cùng một cách, tạo ra k2 từ h2 và d1. 
3. Chuyển số lần trúng đích thành số lần chết bằng cách sử dụng khoảng thời gian bắn. Người chơi thứ hai giết từng người một (k1 − 1)·t2 + 0,5, vì lần đánh đầu tiên tiếp đất sau lần bắn đầu tiên và mỗi lần đánh tiếp theo cách nhau một t2. Người chơi một giết người chơi thứ hai cùng một lúc (k2 − 1)·t1 + 0,5. 
4. So sánh hai lần này. Nếu lần đầu tiên nhỏ hơn, người chơi thứ hai tung ra đòn chí mạng sớm hơn, do đó người chơi thứ hai thắng. Nếu lần thứ hai nhỏ hơn, người chơi thứ nhất sẽ thắng. 
5. Nếu thời gian bằng nhau, cả hai đơn vị đều nhận sát thương chí mạng cuối cùng cùng một lúc và không ai sống sót lâu hơn, do đó kết quả là hòa. 

Tại sao nó hoạt động được gắn liền với thực tế là quá trình gây sát thương của mỗi đơn vị là một cấp số cộng thống nhất theo thời gian. Mọi sự kiện liên quan chỉ được xác định bởi chỉ số của cú đánh gây tử vong. Vì tất cả các đòn đánh đều có cùng độ trễ cố định, nên thứ tự tương đối chỉ phụ thuộc vào số khoảng thời gian trước đòn kết liễu chứ không phụ thuộc vào bất kỳ tương tác trung gian nào giữa hai dòng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    h1, d1, t1 = map(int, input().split())
    h2, d2, t2 = map(int, input().split())

    def hits_needed(h, d):
        return (h + d - 1) // d

    k1 = hits_needed(h1, d2)
    k2 = hits_needed(h2, d1)

    time_2_kills_1 = (k1 - 1) * t2
    time_1_kills_2 = (k2 - 1) * t1

    if time_1_kills_2 < time_2_kills_1:
        print("player one")
    elif time_2_kills_1 < time_1_kills_2:
        print("player two")
    else:
        print("draw")

if __name__ == "__main__":
    solve()
```Việc triển khai cốt lõi làm giảm mọi thứ thành hai phép so sánh số nguyên. Hàm trợ giúp tính toán phép chia trần một cách an toàn bằng cách sử dụng số học số nguyên, tránh các vấn đề về dấu phẩy động. 

Độ trễ di chuyển 0,5 giây được cố ý bỏ qua trong so sánh vì nó giống hệt nhau đối với đòn kết liễu cuối cùng của cả hai người chơi và do đó không ảnh hưởng đến thứ tự. Việc bao gồm nó sẽ chỉ thêm một sự thay đổi liên tục như nhau vào cả hai thời gian được tính toán. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên: 

Người chơi thứ nhất có lực sát thương yếu hơn nhưng có sự cân bằng thời gian thay đạn khác. Lượng sát thương gây ra trên mỗi đòn đánh của người chơi thứ hai cao hơn, điều này làm giảm số lượng đòn đánh cần thiết. 

| Số lượng | Người chơi 1 giết Người chơi 2 | Người chơi 2 giết Người chơi 1 | 
| --- | --- | --- | 
| Số lượt truy cập cần thiết | trần(30/15) = 2 | trần(30/10) = 3 | 
| Giết thời gian | (2−1)·19 = 19 | (3−1)·10 = 20 | 

Thời gian tiêu diệt của người chơi một là 19, trong khi người chơi thứ hai cần 20. Mặc dù người chơi một có sát thương trên mỗi đòn đánh thấp hơn nhưng cần ít lượt đánh hơn, do đó, phát bắn chết người của người chơi một sẽ đến sớm hơn, phù hợp với người chiến thắng dự kiến. 

Bây giờ hãy xem xét trường hợp hoán đổi giống đối xứng: 

| Số lượng | Người chơi 1 giết Người chơi 2 | Người chơi 2 giết Người chơi 1 | 
| --- | --- | --- | 
| Số lượt truy cập cần thiết | trần(30/10) = 3 | trần(30/15) = 2 | 
| Giết thời gian | (3−1)·10 = 20 | (2−1)·19 = 19 | 

Ở đây người chơi thứ hai chết sớm hơn ở thời điểm 19, vì vậy người chơi thứ hai thắng. 

Những ví dụ này cho thấy người chiến thắng phụ thuộc nhiều hơn vào sự tương tác giữa tốc độ tải lại và số lần trúng yêu cầu hơn là chỉ dựa vào giá trị sát thương thô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép tính số học và so sánh được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giới hạn đầu vào cho phép nhiều sự kiện kích hoạt tiềm năng, nhưng giải pháp tránh liệt kê chúng hoàn toàn. Tất cả động lực được nén thành hai số nguyên được tính toán cho mỗi người chơi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("30 10 10\n30 15 19\n") == "player two"
assert run("30 15 19\n30 10 10\n") == "player one"
assert run("100 20 10\n100 12 5\n") == "draw"

# custom cases

# minimum values, symmetric
assert run("1 1 1\n1 1 1\n") == "draw"

# one-shot kill scenario
assert run("10 10 5\n10 1 5\n") == "player one"

# strong damage but slow reload vs weak fast attacker
assert run("100 50 100\n100 10 1\n") == "player two"

# equal death timing edge
assert run("30 10 10\n30 10 10\n") == "draw"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 / 1 1 1 | vẽ | trường hợp tầm thường đối xứng | 
| 10 10 5 / 10 1 5 | người chơi một | tiêu diệt một phát bất đối xứng | 
| 100 50 100 / 100 10 1 | người chơi hai | tải lại sự thống trị về sát thương | 

## Vỏ cạnh 

Một trường hợp phạt góc quan trọng xảy ra khi cả hai đơn vị đều chết ở cùng một chỉ số bắn. Cân nhắc lượng máu bằng nhau và sát thương/tải lại đối xứng. Cả hai đều tính toán số lần bắn giống hệt nhau và lịch bắn giống hệt nhau, tạo ra thời gian tiêu diệt giống hệt nhau. Thuật toán so sánh các khoảng thời gian bằng nhau và trả về một kết quả hòa chính xác, phù hợp với thực tế là cả hai sự kiện tử vong đều xảy ra tại cùng một thời điểm do độ trễ 0,5 giây giống hệt nhau. 

Một trường hợp tinh vi khác là khi một đơn vị gây sát thương cực cao và chỉ cần một đòn là có thể tiêu diệt đối thủ. Trong tình huống này, thời gian tính toán trở thành 0 cho bên đó, vì (1−1)·t = 0. Điều này mô hình chính xác rằng phát bắn đầu tiên ngay lập tức xác định kết quả và bất kỳ kế hoạch tiêu diệt chậm hơn nào của đối thủ đều không thể can thiệp trước khi việc so sánh thời gian tác động đầu tiên được giải quyết. 

Trường hợp cuối cùng là thời gian tải lại khác nhau đáng kể. Một đơn vị có khả năng tải lại rất chậm nhưng sát thương cao vẫn có thể thua đơn vị nhanh có sát thương thấp vì số lần bắn trúng yêu cầu sẽ tăng số khoảng thời gian, đẩy đòn đánh cuối cùng của nó muộn hơn. Thuật toán xử lý việc này một cách tự nhiên vì cả hai yếu tố đều được mã hóa theo cùng một biểu thức tuyến tính cho thời gian chết.
