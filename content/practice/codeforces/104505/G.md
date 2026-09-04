---
title: "CF 104505G - Anh hùng lựa chọn"
description: "Chúng tôi được đưa ra một chuỗi các cấp độ được chơi theo một thứ tự cố định. Khi bắt đầu, một anh hùng có một số sức mạnh ban đầu và ở mỗi cấp độ có sẵn hai quái vật."
date: "2026-06-30T10:58:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104505
codeforces_index: "G"
codeforces_contest_name: "2023 USP Try-outs"
rating: 0
weight: 104505
solve_time_s: 59
verified: true
draft: false
---

[CF 104505G - Anh hùng lựa chọn](https://codeforces.com/problemset/problem/104505/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một chuỗi các cấp độ được chơi theo một thứ tự cố định. Khi bắt đầu, một anh hùng có một số sức mạnh ban đầu và ở mỗi cấp độ có sẵn hai quái vật. Người chơi phải chọn chính xác một quái vật cho mỗi cấp độ và hạn chế duy nhất là sức mạnh của quái vật được chọn không thể vượt quá sức mạnh hiện tại của anh hùng. 

Sau khi đánh bại một con quái vật, sức mạnh của anh hùng sẽ tăng lên đúng bằng sức mạnh của con quái vật đó. Điều này tạo ra một vòng phản hồi: việc chọn sớm một quái vật yếu hơn có thể ngăn cản việc mở khóa các lựa chọn mạnh hơn sau này, trong khi việc chọn một quái vật mạnh hơn hiện có có thể là không thể nếu sức mạnh hiện tại quá nhỏ. 

Nhiệm vụ không phải là tính toán sức mạnh cuối cùng hay điểm số tối ưu mà chỉ để xác định tính khả thi: liệu có tồn tại bất kỳ chuỗi lựa chọn nào, mỗi lựa chọn cho mỗi cấp độ, cho phép anh hùng sống sót ở tất cả các cấp độ theo thứ tự hay không. 

Các ràng buộc cho phép lên tới 2000 cấp độ, với các giá trị lên tới 10^6. Một tìm kiếm hàm mũ đơn giản đối với các lựa chọn sẽ ngay lập tức trở nên quá lớn vì mỗi cấp độ phân nhánh thành hai khả năng, dẫn đến 2^n đường dẫn khả thi, vượt xa mọi tính toán khả thi. 

Một trường hợp thất bại tinh vi đối với trực giác tham lam xuất hiện khi một con quái vật mạnh hơn ở địa phương chặn khả năng tiếp cận trong tương lai. Ví dụ: nếu việc chọn một con quái vật lớn sớm khiến cấp độ sau không thể thực hiện được vì cả hai quái vật đều vượt quá sức mạnh còn lại, thì chiến lược tham lam “luôn lấy thứ mạnh nhất có thể” có thể thất bại. Ngược lại, luôn lấy điểm yếu nhất có thể cũng có thể thất bại vì nó có thể không tăng đủ sức mạnh để mở khóa các lựa chọn trong tương lai. 

Vì vậy, khó khăn nằm ở việc theo dõi tất cả các trạng thái sức mạnh có thể tiếp cận sau mỗi cấp độ đồng thời tránh sự bùng nổ theo cấp số nhân. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force coi đây là tìm kiếm đường dẫn trong biểu đồ phân lớp. Mỗi lớp tương ứng với một cấp độ và mỗi trạng thái có thể là một sức mạnh anh hùng. Từ trạng thái có sức mạnh x ở cấp độ i, chúng ta có thể chuyển sang cấp độ i+1 bằng cách chọn quái vật a_i hoặc b_i, với điều kiện là ≤ x và trạng thái mới trở thành x + a_i hoặc x + b_i. 

Điều này ngay lập tức gợi ý BFS hoặc DFS trên các trạng thái. Vấn đề là số lượng các giá trị công suất riêng biệt tăng lên nhanh chóng. Trong trường hợp xấu nhất, mọi lựa chọn đều tạo ra một tổng riêng biệt mới, vì vậy sau cấp độ i, chúng ta có thể có trạng thái O(2^i). Với n = 2000 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mặc dù số lượng chuỗi là theo cấp số nhân, cấu trúc của các giá trị lũy thừa có thể tiếp cận có đặc tính đơn điệu mạnh mẽ: đối với một mức cố định, nếu có thể tiếp cận được một lũy thừa nhất định thì tất cả các lũy thừa nhỏ hơn cũng bị chi phối một cách hiệu quả và không cần thiết phải theo dõi riêng lẻ. Điều này cho phép chúng tôi nén không gian trạng thái thành công suất tối đa có thể tiếp cận sau mỗi cấp độ, hay chính xác hơn là một tập hợp “đại diện tốt nhất” có thể được giảm xuống thành một biên giới tham lam duy nhất. 

Ở mỗi cấp độ, chúng ta không cần phải theo dõi tất cả các quyền hạn có thể có. Chúng tôi chỉ quan tâm đến sức mạnh tối đa có thể đạt được cho đến nay, bởi vì bất kỳ trạng thái nào có sức mạnh nhỏ hơn đều không thể mở khóa bất cứ thứ gì mà trạng thái tối đa cũng không thể mở khóa ở cùng cấp độ. Vì quá trình chuyển đổi chỉ yêu cầu kiểm tra tính khả thi (x ≥ a_i hoặc b_i), quy trình giảm xuống còn duy trì một giá trị: công suất hiện tại tốt nhất có thể sau khi xử lý từng cấp độ, giả sử chúng ta luôn chọn phương án khả thi nhất. 

Việc quyết định ở mỗi cấp độ trở nên đơn giản: giữa a_i và b_i, chúng ta lấy giá trị lớn nhất ≤ công suất dòng điện. Nếu cả hai đều không khả thi, chúng ta sẽ thất bại. 

Sự tham lam này có tác dụng vì việc chọn một con quái vật khả thi lớn hơn không bao giờ làm giảm các lựa chọn trong tương lai so với việc chọn một con quái vật nhỏ hơn. Cả hai lựa chọn đều tiêu tốn một cấp độ, nhưng lựa chọn lớn hơn sẽ tăng cường nghiêm ngặt hoặc duy trì ngưỡng có thể đạt được trong tương lai mạnh mẽ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Mô phỏng tham lam | O(n) | O(1) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cấp độ theo thứ tự trong khi duy trì một biến duy nhất`power`, đại diện cho sức mạnh anh hùng tối đa có thể đạt được ở cấp độ hiện tại. 

1. Khởi tạo`power = f`, cường độ ban đầu. Điều này thể hiện trạng thái tốt nhất có thể trước khi đưa ra bất kỳ quyết định nào. 
2. Với mỗi cấp độ i từ 1 đến n, hãy kiểm tra hai con quái vật`a_i`Và`b_i`. 
3. Kiểm tra xem trong hai quái vật nào có thể bị đánh bại, nghĩa là sức mạnh của chúng ở mức ≤ hiện tại`power`. 
4. Nếu không có con quái vật nào có thể bị đánh bại, hãy kết luận ngay rằng trò chơi không thể hoàn thành. 
5. Nếu chính xác một quái vật có thể bị đánh bại, hãy chọn nó và thêm sức mạnh của nó vào`power`. 
6. Nếu cả hai đều có thể đánh bại được, hãy chọn cái lớn hơn. Điều này tối đa hóa sự gia tăng trong`power`với cùng một mức chi phí tiêu dùng, đảm bảo không có bất lợi nào trong tương lai. 
7. Sau khi xử lý thành công tất cả các cấp độ, xuất ra thông báo rằng trò chơi có thể hoàn thành. 

### Tại sao nó hoạt động 

Quá trình này dựa trên thực tế là ở mọi cấp độ, chỉ có sức mạnh tối đa hiện tại có thể đạt được mới là quan trọng. Nếu hai trạng thái tồn tại ở cùng cấp độ, thì trạng thái có quyền lực cao hơn sẽ thống trị trạng thái kia vì mọi ràng buộc trong tương lai đều có dạng “sức mạnh ít nhất phải có ngưỡng nào đó để tiếp tục”. Vì quá trình chuyển đổi chỉ làm tăng công suất nên giá trị hiện tại cao hơn không bao giờ có thể làm giảm tính khả thi trong tương lai. Do đó, chỉ duy trì sức mạnh tối đa có thể tiếp cận là đủ và việc tham lam lựa chọn con quái vật khả thi lớn nhất sẽ duy trì mức tối đa này ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, f = map(int, input().split())
    power = f

    for _ in range(n):
        a, b = map(int, input().split())

        can_a = a <= power
        can_b = b <= power

        if not can_a and not can_b:
            print("N")
            return

        if can_a and can_b:
            chosen = max(a, b)
        else:
            chosen = a if can_a else b

        power += chosen

    print("S")

if __name__ == "__main__":
    solve()
```Giải pháp giữ một giá trị đang chạy`power`và cập nhật nó theo cấp độ. Chi tiết triển khai quan trọng là chúng tôi luôn kiểm tra tính khả thi trước khi chọn, đảm bảo rằng chúng tôi không bao giờ thêm quái vật không hợp lệ. Khi cả hai lựa chọn đều hợp lệ, chúng tôi sẽ tận dụng mức tối đa một cách rõ ràng để tối đa hóa khả năng tiếp cận trong tương lai. 

Thứ tự kiểm tra rất quan trọng: chúng ta phải phát hiện trường hợp thất bại trước khi cố gắng chọn một con quái vật. Nếu không, những bổ sung không hợp lệ có thể tăng sức mạnh một cách không chính xác và che giấu sự bất khả thi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1 2
5 3
4 4
```| Cấp độ | quyền lực trước | một | b | lựa chọn khả thi | đã chọn | sức mạnh sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 2 | cả hai | 2 | 4 | 
| 2 | 4 | 5 | 3 | chỉ 3 | 3 | 7 | 
| 3 | 7 | 4 | 4 | cả hai | 4 | 11 | 

Ở mỗi bước, có ít nhất một quái vật. Cấp độ cuối cùng thành công, vì vậy đầu ra là`S`. 

### Mẫu 2 

đầu vào:```
3 2
4 4
1 2
5 3
```| Cấp độ | quyền lực trước | một | b | lựa chọn khả thi | đã chọn | sức mạnh sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 4 | 4 | không | thất bại | - | 

Cấp độ đầu tiên đã có cả quái vật mạnh hơn anh hùng nên không thể di chuyển được và câu trả lời là`N`. 

Ví dụ thứ hai cho thấy tình trạng chặn ngay lập tức, trong đó lỗi xảy ra trước bất kỳ quá trình phát triển trạng thái nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cấp độ được xử lý bằng các phép so sánh và số học liên tục theo thời gian | 
| Không gian | O(1) | Chỉ có một biến duy nhất`power`được duy trì | 

Với n lên đến 2000, thời gian tuyến tính nhanh hơn rất nhiều trong giới hạn và mức sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume function is imported
    return solve()

# provided samples
assert run("3 2\n1 2\n5 3\n4 4\n") == "S"
assert run("3 2\n4 4\n1 2\n5 3\n") == "N"

# minimum size success
assert run("1 5\n3 4\n") == "S"

# minimum size failure
assert run("1 3\n4 5\n") == "N"

# all equal and always feasible
assert run("4 1\n1 1\n1 1\n1 1\n1 1\n") == "S"

# case where choice matters but greedy still works
assert run("3 3\n2 3\n4 1\n5 2\n") == "S"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thành công nhỏ duy nhất | S | tính khả thi cơ bản | 
| đơn không thể | N | thất bại ngay lập tức | 
| giá trị bằng nhau lặp đi lặp lại | S | sự ổn định dưới mối quan hệ | 
| ràng buộc hỗn hợp | S | tính nhất quán tham lam | 

## Vỏ cạnh 

Trường hợp quan trọng là khi cả hai quái vật đều bằng sức mạnh hiện tại. Thuật toán vẫn phải coi điều này là hợp lệ và cho phép lựa chọn. Ví dụ: 

đầu vào:```
2 3
3 3
3 3
```Ở cấp độ 1, cả hai đều khả thi, vì vậy chúng tôi chọn 3 và tăng sức mạnh lên 6. Ở cấp độ 2, một lần nữa cả hai đều khả thi và chúng tôi thành công. Quy tắc tham lam không thành vấn đề vì cả hai lựa chọn đều giống nhau. 

Một trường hợp khác là bế tắc ngay lập tức: 

đầu vào:```
1 2
3 3
```Ở cấp độ duy nhất, cả hai quái vật đều vượt quá sức mạnh, do đó việc kiểm tra sẽ gây ra lỗi một cách chính xác trước bất kỳ bản cập nhật nào. 

Trường hợp tinh tế thứ ba là khi một con quái vật khả thi nhưng nhỏ hơn, còn con quái vật kia thì không khả thi: 

đầu vào:```
1 5
2 10
```Chỉ có 2 là hợp lệ, vì vậy chúng ta phải chọn nó. Chọn mức tối đa mà không kiểm tra tính khả thi sẽ chọn sai 10 và chấp nhận trường hợp sai. Việc kiểm tra tính khả thi trước khi lấy mức tối đa là điều cần thiết để đảm bảo tính chính xác.
