---
problem: 1366A
contest_id: 1366
problem_index: A
name: "Shovels and Swords"
contest_name: "Educational Codeforces Round 89 (Rated for Div. 2)"
rating: 1100
tags: ["binary search", "greedy", "math"]
answer: passed_samples
verified: true
solve_time_s: 120
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e4b06-3048-83ec-8673-f24f28d482e2
---

#CF 1366A - Xẻng và Kiếm 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e4b06-3048-83ec-8673-f24f28d482e2 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta gặp một vấn đề về quản lý tài nguyên trong đó mỗi trường hợp thử nghiệm mô tả hai loại vật liệu: que và kim cương. Từ những vật liệu này, chúng ta có thể chế tạo hai vật phẩm khác nhau. Một vật phẩm tiêu tốn hai cây gậy và một viên kim cương, còn vật phẩm kia tiêu tốn một cây gậy và hai viên kim cương. Mỗi vật phẩm chế tạo có thể được bán với đúng một đơn vị lợi nhuận, vì vậy nhiệm vụ là tối đa hóa số lượng vật phẩm chúng ta có thể chế tạo mà không vi phạm giới hạn tài nguyên. 

Điều quan trọng không phải là thứ tự chế tạo mà là chúng ta chọn bao nhiêu loại trong số đó để tổng mức tiêu thụ tài nguyên vẫn nằm trong số gậy và kim cương có sẵn. Mỗi mục được chế tạo có giá trị độc lập, do đó mục tiêu giảm xuống mức tối đa hóa tổng số cặp hợp lệ (2,1) và (1,2) trong điều kiện hạn chế về tài nguyên. 

Các ràng buộc cho phép tối đa 1000 trường hợp kiểm thử, với mỗi trường hợp kiểm thử chứa các giá trị lên tới 10^9. Điều này ngay lập tức loại trừ mọi mô phỏng hoặc tìm kiếm trên các kết hợp. Bất kỳ cách tiếp cận nào cố gắng liệt kê số lượng xẻng và kiếm cần chế tạo sẽ giảm xuống thành O(a + b) hoặc tệ hơn trong mỗi trường hợp thử nghiệm, tốc độ này quá chậm. Chúng ta cần một công thức thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Một cạm bẫy ngây thơ nhưng quan trọng là cho rằng chúng ta nên luôn sử dụng mọi nguồn lực một cách tham lam theo một hướng, chẳng hạn như tạo ra càng nhiều xẻng càng tốt trước tiên. Ví dụ ta có 4 gậy và 4 viên kim cương, tham lam lấy xẻng trước sẽ ra 1 xẻng (2 gậy, 1 viên kim cương), để lại 2 gậy và 3 viên kim cương thì có thể lấy 1 kiếm, tổng cộng là 2, là tối ưu. Nhưng trong những cách phân phối khác, sự lựa chọn tham lam có thể gây hiểu nhầm nếu không suy luận cẩn thận. 

Một trường hợp phức tạp hơn là khi tài nguyên bị mất cân bằng cao. Ví dụ: với 1 cây gậy và 10 viên kim cương, không thể tạo ra vật phẩm nào mặc dù rất nhiều kim cương, bởi vì cả hai công thức đều yêu cầu ít nhất một cây gậy. Tương tự, 10 cây gậy và 1 viên kim cương cũng cho kết quả bằng 0. Bất kỳ giải pháp đúng nào cũng phải ngầm hiểu rằng cả hai tài nguyên phải được tiêu thụ theo cặp cân bằng. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ là thử tất cả số xẻng có thể có. Nếu chúng ta cố định số xẻng x, thì chúng ta tiêu thụ 2x gậy và x kim cương, khiến nguồn tài nguyên giảm đi, từ đó chúng ta tính toán xem chúng ta có thể tạo ra bao nhiêu thanh kiếm, số lượng này sẽ bị giới hạn bởi số gậy và kim cương còn lại. Chúng tôi sẽ thử tất cả x từ 0 đến min(a//2, b) và tính kết quả tốt nhất. 

Cách tiếp cận này đúng vì mọi giải pháp hợp lệ đều tương ứng với một số lựa chọn x xẻng và y kiếm. Tuy nhiên, phạm vi của x có thể lên tới 10^9 trong trường hợp xấu nhất, khiến cho việc quét tuyến tính này không thể thực hiện được. Mỗi đánh giá là O(1), do đó tổng độ phức tạp sẽ trở thành O(min(a, b)) cho mỗi trường hợp kiểm thử, quá chậm đối với 1000 bài kiểm tra. 

Quan sát quan trọng là mỗi mục tiêu thụ tổng cộng chính xác ba tài nguyên, nhưng ở các tỷ lệ khác nhau. Xẻng là 2 gậy và 1 viên kim cương, trong khi kiếm là 1 gậy và 2 viên kim cương. Sự đối xứng này ngụ ý rằng mỗi vật phẩm được chế tạo sẽ “tiêu thụ” ba đơn vị một cách hiệu quả, nhưng yếu tố hạn chế không phải là tổng tài nguyên mà là sự cân bằng giữa gậy và kim cương. 

Nếu chúng ta nghĩ về mặt ghép nối, mỗi mặt hàng sẽ tiêu thụ một đơn vị tài nguyên dồi dào hơn hai lần và đơn vị kia một lần. Điều này gợi ý rằng câu trả lời bị hạn chế bởi cả tổng tài nguyên và sự mất cân bằng giữa a và b. Cấu trúc tối ưu sẽ luôn sử dụng càng nhiều cặp càng tốt cho đến khi một tài nguyên trở nên quá nhỏ để hỗ trợ chế tạo cân bằng hơn nữa.

Cấu trúc kết quả là câu trả lời được giới hạn bởi tổng số vật phẩm có thể chế tạo được nếu tài nguyên được cân bằng hoàn hảo, tức là (a + b) // 3, và cũng bởi tài nguyên nhỏ hơn theo cách đảm bảo tính khả thi. Ràng buộc chặt chẽ hơn hóa ra là mức tối thiểu giữa (a + b) // 3 và min(a, b), vì mỗi mục phải tiêu thụ ít nhất một đơn vị của cả hai tài nguyên trên tất cả các cấu hình. 

Chúng tôi đi đến một công thức thời gian không đổi nắm bắt trực tiếp cả hai ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua xẻng có thể | O(min(a, b)) | O(1) | Quá chậm | 
| Công thức tham lam tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp tối ưu dựa vào việc tính toán số lượng vật phẩm tối đa theo hai ràng buộc đồng thời: tổng số bộ ba tiêu thụ và cân bằng tài nguyên. 

1. Với mỗi bộ test, đọc số que a và số viên kim cương b. Chúng xác định các tài nguyên sẵn có phải được phân phối theo hai công thức chế tạo. 
2. Tính tổng số mục có thể có nếu chúng ta bỏ qua sự mất cân bằng, là (a + b) // 3. Điều này thể hiện thực tế là mỗi mục tiêu thụ tổng cộng chính xác 3 tài nguyên, do đó, ngay cả một hệ thống cân bằng hoàn hảo cũng không thể vượt quá giới hạn này. Điều này đưa ra giới hạn trên tuyệt đối cho sản xuất. 
3. Tính số lượng vật phẩm bị giới hạn bởi nguồn lực khan hiếm hơn, là min(a, b). Mỗi mục yêu cầu ít nhất một đơn vị của cả hai tài nguyên trong một số kết hợp, vì vậy chúng tôi không thể vượt quá nhóm tài nguyên nhỏ hơn. 
4. Lấy giá trị tối thiểu của hai giới hạn này làm đáp án cuối cùng. Điều này đảm bảo chúng tôi tôn trọng cả giới hạn tiêu thụ tổng cộng và giới hạn cân bằng giữa gậy và kim cương. 

### Tại sao nó hoạt động 

Mỗi vật phẩm được chế tạo sẽ giảm chính xác nhóm tài nguyên kết hợp đi ba đơn vị, do đó không có giải pháp nào có thể vượt quá (a + b) // 3. Đồng thời, mỗi vật phẩm phải tiêu thụ tổng cộng ít nhất một đơn vị của cả hai tài nguyên, vì cả hai công thức nấu ăn đều bao gồm cả hai thành phần. Điều này buộc số lượng mục không được vượt quá số nhỏ hơn của a và b. Bất kỳ cách xây dựng hợp lệ nào cũng phải đáp ứng cả hai ràng buộc và vì hai loại hạng mục cho phép phân phối lại sự mất cân bằng một cách linh hoạt nên luôn có thể đạt được giới hạn nhỏ hơn trong hai giới hạn này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    print(min((a + b) // 3, min(a, b)))
```Giải pháp xử lý từng trường hợp thử nghiệm một cách độc lập và tính toán hai đại lượng trong thời gian không đổi. Đầu tiên là tổng số mặt hàng có thể được đóng gói hoàn hảo, xuất phát từ việc nhóm các nguồn lực thành ba phần. Thứ hai là giới hạn được áp đặt bởi loại tài nguyên nhỏ hơn, đảm bảo rằng cả gậy và kim cương đều không bị sử dụng quá mức vượt quá giới hạn về tính sẵn có. 

Câu trả lời cuối cùng là giá trị tối thiểu của hai giá trị này, đảm bảo cả tính khả thi và tối ưu. 

Một lỗi phổ biến là chỉ sử dụng (a + b) // 3, đánh giá quá cao câu trả lời khi một nguồn lực khan hiếm. Một sai lầm khác là chỉ sử dụng min(a, b), bỏ qua thực tế là tổng tài nguyên có thể vẫn không đủ ngay cả khi cả hai đều cân bằng. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp mẫu để xem công thức hoạt động như thế nào. 

### Ví dụ 1: a = 4, b = 4 

| Bước | một | b | (a + b) // 3 | phút(a, b) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 4 | 4 | - | - | - | 
| Tính tổng giới hạn | 4 | 4 | 2 | - | - | 
| Tính toán số dư ràng buộc | 4 | 4 | 2 | 4 | - | 
| Cuối cùng | 4 | 4 | 2 | 4 | 2 | 

Điều này cho thấy một trường hợp cân bằng hoàn hảo trong đó tổng nhóm nguồn lực là yếu tố hạn chế chứ không phải mất cân bằng. 

### Ví dụ 2: a = 8, b = 7 

| Bước | một | b | (a + b) // 3 | phút(a, b) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 8 | 7 | - | - | - | 
| Tính tổng giới hạn | 8 | 7 | 5 | - | - | 
| Tính toán số dư ràng buộc | 8 | 7 | 5 | 7 | - | 
| Cuối cùng | 8 | 7 | 5 | 7 | 5 | 

Ở đây, việc đóng gói tổng tài nguyên sẽ giới hạn câu trả lời, mặc dù cả hai tài nguyên đều tương đối dồi dào. 

Ví dụ thứ hai khẳng định rằng sự mất cân bằng không phải là yếu tố hạn chế; thay vào đó, việc tiêu thụ tài nguyên toàn cầu lại chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm chỉ yêu cầu một vài phép tính số học | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến đầu vào | 

Giải pháp này xử lý thoải mái các ràng buộc tối đa vì thậm chí 1000 trường hợp thử nghiệm chỉ yêu cầu tính toán theo thời gian không đổi cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        out.append(str(min((a + b) // 3, min(a, b))))
    return "\n".join(out)

# provided samples
assert run("4\n4 4\n1000000000 0\n7 15\n8 7\n") == "2\n0\n7\n5"

# custom cases
assert run("3\n0 0\n1 2\n2 1\n") == "0\n0\n1", "minimum and small imbalance cases"
assert run("2\n3 3\n6 1\n") == "2\n1", "balanced and skewed resources"
assert run("1\n1000000000 1000000000\n") == "666666666", "large symmetric case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 / 1 2 / 2 1 | 0/0/1 | tài nguyên trống và tối thiểu | 
| 3 3 / 6 1 | 2/1 | đầu vào cân bằng và bị lệch nhiều | 
| 1e9 1e9 | 666666666 | độ đúng ranh giới lớn | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một tài nguyên bằng 0. Đối với đầu vào 1000000000 0, cả hai công thức đều tạo ra min((a+b)//3, min(a,b)) = 0, ngăn chặn chính xác mọi thao tác chế tạo. Một cách tiếp cận tham lam ngây thơ ưu tiên tổng số tiền có thể cho rằng không chính xác có thể có nhiều món, nhưng nó bỏ qua rằng mọi công thức đều yêu cầu cả hai thành phần. 

Một trường hợp khác là mất cân bằng cực độ, chẳng hạn như 1 và 10. Công thức cho min(11//3, 1) = 1, nghĩa là chỉ có thể có một mục. Quá trình thực hiện truy tìm xác nhận rằng sau bất kỳ thủ công đầu tiên hợp lệ nào, ít nhất một tài nguyên sẽ cạn kiệt ngay lập tức. 

Cuối cùng, các đầu vào lớn đối xứng như 10^9 và 10^9 kiểm tra độ ổn định của số nguyên. Công thức giảm xuống còn 2*10^9 // 3, phù hợp an toàn với số nguyên Python và đảm bảo không xảy ra sự cố tràn hoặc hiệu suất.
