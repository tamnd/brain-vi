---
problem: 1337B
contest_id: 1337
problem_index: B
name: "Kana and Dragon Quest game"
contest_name: "Codeforces Round 635 (Div. 2)"
rating: 900
tags: ["greedy", "implementation", "math"]
answer: passed_samples
verified: true
solve_time_s: 177
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e13b6-5050-83ec-8d1e-01d73bf1343c
---

#CF 1337B - Trò chơi Kana và Dragon Quest 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e13b6-5050-83ec-8d1e-01d73bf1343c 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống theo lượt trong đó một con quái vật bắt đầu với một số giá trị sức khỏe và hai loại hành động có thể được áp dụng lặp đi lặp lại. Mỗi hành động sẽ thay đổi sức khỏe theo một cách xác định: một hành động gần như giảm một nửa sức khỏe nhưng sau đó thêm một hằng số cố định, trong khi hành động kia trừ đi một hằng số cố định. 

Mục đích là để xác định xem có tồn tại bất kỳ chuỗi nào của các hành động này hay không, tôn trọng giới hạn trên về số lần mỗi hành động có thể được sử dụng, điều này cuối cùng sẽ đẩy sức khỏe về 0 hoặc thấp hơn. 

Mỗi trường hợp thử nghiệm đưa ra một giá trị sức khỏe ban đầu và hai bộ đếm biểu thị số lần mỗi phép thuật có thể được sử dụng. Chúng ta phải quyết định xem liệu một số thứ tự của các hoạt động bị giới hạn này có thể làm cho sức khỏe không tích cực hay không. 

Các ràng buộc nhỏ về số lượng phép toán, vì cả hai giới hạn đều nhiều nhất là 30. Điều này ngay lập tức loại trừ mọi nhu cầu tìm kiếm đồ thị phức tạp trên các không gian trạng thái lớn. Ngay cả việc khám phá trạng thái trên tất cả số lượng phép thuật có thể có cũng khả thi trong trường hợp xấu nhất vì số lượng kết hợp nhiều nhất là 31 x 31. Tuy nhiên, phần khó khăn là việc sắp xếp thứ tự rất quan trọng, vì một thao tác thu nhỏ giá trị một cách phi tuyến tính trong khi thao tác kia trừ tuyến tính. 

Cạm bẫy chính là giả định rằng một trật tự tham lam cố định luôn hoạt động mà không cần biện minh. Ví dụ: luôn áp dụng phép giảm một nửa trước có thể thất bại: 

đầu vào:```
x = 64, n = 1, m = 3
```Nếu áp dụng Hấp thụ Void trước, chúng ta sẽ nhận được 64 → 42, sau đó là ba đòn đánh: 12 → 2 → -8 → -18, hiệu quả. Nhưng việc đảo ngược các lựa chọn không chính xác trong những trường hợp tương tự có thể dễ dàng giữ trạng thái trên 0. Sự tương tác giữa giảm một nửa và phép trừ không đơn điệu theo nghĩa đơn giản. 

Một trường hợp thất bại khác xuất hiện khi việc giảm một nửa trở nên có hại: 

đầu vào:```
x = 10, n = 1, m = 0
```Hấp thụ Hư Không mang lại tầng (10/2)+10 = 15, tăng máu thay vì giảm. Một chiến lược ngây thơ luôn sử dụng Hấp thụ Hư Không có sẵn sẽ ngay lập tức khiến tình hình trở nên tồi tệ hơn. 

Vì vậy vấn đề mấu chốt là phép thuật không mang lại lợi ích thống nhất và sự lựa chọn tốt nhất phụ thuộc vào mức độ sức khỏe hiện tại. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả các chuỗi hoạt động có thể lên tới n + m. Ở mỗi bước, nó sẽ phân nhánh tùy thuộc vào việc chúng ta áp dụng Hấp thụ hư không hay Tấn công sét, tôn trọng số lượng còn lại. Điều này đúng vì nó khám phá toàn bộ cây quyết định. Tuy nhiên, hệ số phân nhánh dẫn đến trạng thái khoảng 2^(n+m), trong trường hợp xấu nhất đạt tới 2^60, vượt xa giới hạn khả thi. 

Quan sát quan trọng là giá trị sức khỏe có xu hướng giảm đi khi nó trở nên nhỏ và chế độ nguy hiểm duy nhất là khi Hấp thụ Hư Không có thể tạm thời tăng hoặc hầu như không giảm giá trị. Khi máu đã lớn thì chỉ sử dụng Lightning Strike là hợp lý vì giảm một nửa cộng 10 thường tệ hơn là trừ 10 liên tục. Nhưng khi sức khỏe trở nên nhỏ, việc áp dụng Void Absorb không bao giờ hữu ích nữa vì nó bắt đầu tăng giá trị hoặc giảm quá chậm so với phép trừ trực tiếp. 

Cấu trúc này gợi ý một mô phỏng tham lam với việc cắt tỉa: chúng tôi luôn ưu tiên Lightning Strike khi có thể nếu lượng máu đủ lớn và chỉ sử dụng Hấp thụ Hư Không khi nó giúp giảm cường độ hoặc khi các đòn đánh đã cạn kiệt. Vì n và m nhỏ nên chúng ta có thể mô phỏng an toàn tất cả các chuyển đổi có ý nghĩa mà không cần lo lắng về chuỗi dài. 

Một cách rõ ràng để chính thức hóa điều này là thử áp dụng Lightning Strike một cách tham lam nhiều lần bất cứ khi nào nó có lợi và thỉnh thoảng áp dụng Hấp thụ Void để giảm các giá trị lớn nhanh hơn, nhưng không bao giờ nhiều hơn mức cần thiết. Vì không gian trạng thái nhỏ nên chúng ta có thể mô phỏng một cách an toàn cho đến khi không thể cải thiện thêm hoặc hết giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n+m)) | O(n+m) | Quá chậm | 
| Mô phỏng tham lam | O(n+m) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quy trình cho mỗi trường hợp thử nghiệm, luôn cố gắng giảm thiểu tình trạng một cách hiệu quả nhất có thể. 

1. Nếu chúng tôi vẫn còn sẵn các công dụng Hấp thụ Hư Không và việc áp dụng nó sẽ làm giảm lượng máu đáng kể theo cách vẫn hữu ích, chúng tôi sẽ cân nhắc áp dụng nó khi lượng máu lớn. 

Phép biến đổi có thể giảm hoặc tăng giá trị tùy theo độ lớn, do đó nó phải được sử dụng một cách chọn lọc. 
2. Mặt khác, chúng tôi ưu tiên Lightning Strike, vì nó hoàn toàn đơn điệu và luôn giảm máu một lượng cố định. 
3. Chúng tôi lặp lại quá trình quyết định này cho đến khi sức khỏe trở về 0 hoặc âm tính hoặc chúng tôi thực hiện hết cả hai thao tác. 
4. Nếu chúng tôi không thể giảm sức khỏe xuống 0 hoặc thấp hơn sau khi sử dụng hết tất cả các ứng dụng hợp lệ, chúng tôi kết luận rằng điều đó là không thể. 

Ý tưởng chính là hệ thống không có chu kỳ nào có thể được khai thác vô hạn vì mỗi hoạt động đều bị giới hạn và mọi hoạt động đều làm giảm sức khỏe hoặc cuối cùng trở nên tồi tệ hơn so với giải pháp thay thế. Điều này đảm bảo mô phỏng hội tụ nhanh chóng. 

Điều bất biến là ở mỗi bước, chúng tôi duy trì lượng máu tốt nhất có thể đạt được dựa trên các hoạt động còn lại được ưu tiên tham lam: Tia sét luôn được sử dụng khi nó góp phần trực tiếp vào việc giảm thiểu, trong khi Hấp thụ Hư không chỉ được sử dụng khi nó không dẫn đến tình trạng xấu đi có thể tránh được. Do cả hai thao tác đều bị giới hạn và phép biến đổi làm giảm nghiêm trọng không gian tìm kiếm hiệu quả nên mô phỏng không thể bỏ lỡ một chuỗi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, n, m = map(int, input().split())

        # We try a greedy reduction:
        # Use Void Absorption only while it helps reduce large values.
        # Otherwise use Lightning Strike.
        h = x

        # First, try to use Void Absorption when it is clearly beneficial.
        # Empirically, it is useful only when h > 20.
        while n > 0 and h > 20:
            h = h // 2 + 10
            n -= 1

        # Then use all Lightning Strikes
        h -= 10 * m

        if h <= 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh quan sát rằng Hấp thụ Khoảng trống chỉ hữu ích trong chế độ sức khỏe cao. Khi giá trị giảm xuống dưới một ngưỡng nhỏ, việc giảm một nửa cộng 10 không còn hiệu quả so với phép trừ trực tiếp, vì vậy chúng tôi ngừng sử dụng sớm và chuyển tất cả tài nguyên còn lại thành thiệt hại trực tiếp. 

Hành vi ngưỡng là điểm tinh tế quan trọng: nếu không có nó, một chiến lược xen kẽ ngây thơ có thể dao động hoặc lãng phí các hoạt động. Ngưỡng đã chọn đảm bảo chúng tôi không bao giờ áp dụng một phép biến đổi có thể làm tăng hoặc cản trở tiến trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
x = 100, n = 3, m = 4
```| Bước | Hoạt động | Sức khỏe | Còn lại n | Còn lại m | 
| --- | --- | --- | --- | --- | 
| 0 | Bắt đầu | 100 | 3 | 4 | 
| 1 | Vô hiệu | 60 | 2 | 4 | 
| 2 | Vô hiệu | 40 | 1 | 4 | 
| 3 | Vô hiệu | 30 | 0 | 4 | 
| 4 | đình công | 0 | 0 | 0 | 

Sức khỏe trở nên không khả quan, khẳng định sự thành công. Điều này chứng tỏ rằng việc giảm một nửa lặp đi lặp lại ở phạm vi cao sẽ nhanh chóng tăng tốc độ giảm trước khi chuyển sang phép trừ tuyến tính. 

### Ví dụ 2 

đầu vào:```
x = 63, n = 2, m = 3
```| Bước | Hoạt động | Sức khỏe | Còn lại n | Còn lại m | 
| --- | --- | --- | --- | --- | 
| 0 | Bắt đầu | 63 | 2 | 3 | 
| 1 | Vô hiệu | 41 | 1 | 3 | 
| 2 | Vô hiệu | 30 | 0 | 3 | 
| 3 | đình công | 20 | 0 | 2 | 
| 4 | đình công | 10 | 0 | 1 | 
| 5 | đình công | 0 | 0 | 0 | 

Điều này cho thấy điểm chuyển tiếp quan trọng trong đó việc giảm một nửa chỉ được sử dụng khi có tác động và sau đó tất cả các hoạt động còn lại đều là giảm tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi thử nghiệm thực hiện tối đa một số phép toán giới hạn nhỏ, vì n và m nhiều nhất là 30 | 
| Không gian | O(1) | Chỉ một số biến số nguyên được duy trì cho mỗi lần kiểm tra | 

Giải pháp phù hợp thoải mái trong giới hạn vì ngay cả trong trường hợp xấu nhất là 1000 trường hợp thử nghiệm, tổng số thao tác mô phỏng là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            x, n, m = map(int, input().split())
            h = x
            while n > 0 and h > 20:
                h = h // 2 + 10
                n -= 1
            h -= 10 * m
            out.append("YES" if h <= 0 else "NO")
        return "\n".join(out)

    return solve()

# provided samples
assert run("7\n100 3 4\n189 3 4\n64 2 3\n63 2 3\n30 27 7\n10 9 1\n69117 21 2\n") == \
"YES\nNO\nNO\nYES\nYES\nYES\nYES"

# custom cases
assert run("1\n10 0 1\n") == "YES", "only strike"
assert run("1\n10 1 0\n") == "NO", "void increases hp"
assert run("1\n1 30 30\n") == "YES", "overkill"
assert run("1\n100000 0 30\n") == "YES", "pure strikes large hp"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 0 1 | CÓ | đòn kết liễu trực tiếp | 
| 10 1 0 | KHÔNG | sự hấp thụ khoảng trống có thể làm tình trạng xấu đi | 
| 1 30 30 | CÓ | trường hợp quá mức cần thiết | 
| 100000 0 30 | CÓ | trường hợp giảm tuyến tính duy nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi lượng máu nhỏ và Hấp thụ Hư Không sẽ tăng lên. Ví dụ: bắt đầu từ con số 10 mà không có đòn đánh nào và một phép thuật vô hiệu, quá trình biến đổi tạo ra 15, điều này khiến tình hình trở nên tồi tệ hơn và dẫn đến thất bại. Thuật toán tránh điều này bằng cách không bao giờ áp dụng Hấp thụ Void trong chế độ sức khỏe thấp. 

Một trường hợp khác xảy ra khi lượng máu lớn nhưng chỉ có một số phép thuật trống. Trong những trường hợp như vậy, việc áp dụng tất cả các phép thuật trống sớm sẽ có lợi vì nó nhanh chóng đưa giá trị xuống khu vực mà các đòn đánh chiếm ưu thế. Ngưỡng tham lam đảm bảo việc tăng tốc này xảy ra trước khi chuyển đổi chiến lược. 

Trường hợp cuối cùng là khi chỉ tồn tại Lightning Strikes. Sau đó, giải pháp giảm xuống còn kiểm tra xem m * 10 có ít nhất là x hay không, điều này được xử lý chính xác ở bước trừ cuối cùng.
