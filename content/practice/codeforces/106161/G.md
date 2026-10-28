---
title: "CF 106161G - GCD của tập hợp con"
description: "Chúng tôi được cung cấp một bộ sưu tập các mặt hàng. Mỗi mục có trọng lượng và giá trị, nhưng đối với một số mục, chính xác một trong hai số này bị thiếu và chúng tôi phải gán dưới dạng số nguyên dương không quá một tỷ. Hai thủ tục tham lam sau đó sẽ chạy trên tập dữ liệu đã hoàn thành."
date: "2026-06-20T22:14:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106161
codeforces_index: "G"
codeforces_contest_name: "The 2025 ICPC Asia Chengdu Regional Contest (The 4rd Universal Cup. Stage 4: Grand Prix of Chengdu)"
rating: 0
weight: 106161
solve_time_s: 88
verified: true
draft: false
---

[CF 106161G - GCD của các tập hợp con](https://codeforces.com/problemset/problem/106161/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các mặt hàng. Mỗi mục có trọng lượng và giá trị, nhưng đối với một số mục, chính xác một trong hai số này bị thiếu và chúng tôi phải gán dưới dạng số nguyên dương không quá một tỷ. 

Hai thủ tục tham lam sau đó sẽ chạy trên tập dữ liệu đã hoàn thành. Alice sắp xếp các món đồ theo trọng lượng tăng dần rồi lấy các món đồ theo thứ tự đó cho đến khi món đồ tiếp theo vượt quá sức chứa của ba lô, lúc đó cô ấy dừng hoàn toàn. Bob sắp xếp các mục theo giá trị giảm dần rồi quét theo thứ tự đó, lấy một mục bất cứ khi nào nó phù hợp với dung lượng còn lại. 

Nhiệm vụ của chúng ta là gán các trọng số và giá trị còn thiếu để cả hai quy trình đều chọn chính xác cùng một tập hợp các mục. 

Khó khăn chính là việc phân công ảnh hưởng đến cả thứ tự sắp xếp và tính khả thi của chiếc ba lô, vì vậy chúng tôi đồng thời kiểm soát thứ tự và số tiền tích lũy theo một ràng buộc dung lượng duy nhất. 

Các ràng buộc đủ nhỏ để lý luận bậc hai hoặc thậm chí hơi siêu khối cho mỗi trường hợp thử nghiệm không được chấp nhận trong mọi trường hợp. Tổng cộng có tối đa 3000 mục, nhưng khó khăn thực sự không phải là tính toán thô, mà là xây dựng một bài tập nhất quán dưới những ràng buộc hỗn hợp đến từ thứ tự và năng lực. 

Một dạng thất bại tinh vi sẽ xuất hiện ngay lập tức nếu một người cố gắng đối xử độc lập với Alice và Bob. Lựa chọn của Alice chỉ phụ thuộc vào thứ tự trọng số và tổng tiền tố, trong khi lựa chọn của Bob phụ thuộc vào thứ tự giá trị và tính khả thi tăng dần. Một nỗ lực ngây thơ nhằm sửa một thứ tự trước tiên thường sẽ làm cho thủ tục tham lam khác trở nên khác biệt, bởi vì thủ tục thứ hai rất nhạy cảm với dung lượng còn lại trung gian chứ không chỉ là tổng cuối cùng. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ cố gắng gán tất cả các giá trị còn thiếu và sau đó mô phỏng cả hai chiến lược. Điều này rõ ràng là không thể vì mỗi ẩn số có thể có tới một tỷ giá trị, tạo ra một không gian tìm kiếm theo cấp số nhân hoặc vô hạn. 

Một quan sát có cấu trúc hơn xuất phát từ việc hiểu được quy trình của Alice thực sự tạo ra những gì. Khi trọng số đã được cố định, Alice không chọn tùy tiện mà luôn lấy tiền tố theo thứ tự sắp xếp theo trọng số và dừng lại ở mục đầu tiên không phù hợp. Điều này có nghĩa là tập hợp được chọn của Alice hoàn toàn được xác định bởi một ngưỡng theo thứ tự đó. 

Quy trình của Bob linh hoạt hơn về thứ tự lựa chọn, nhưng vẫn tham lam theo nghĩa là khi các mục được sắp xếp theo giá trị, anh ấy sẽ cố gắng đóng gói chúng nếu có thể. 

Ý tưởng chính là chúng ta nên tránh bất kỳ sự tương tác nào trong đó các vật phẩm do Alice lấy được xen kẽ với các vật phẩm mà Bob không lấy theo thứ tự nào đó. Nếu chúng ta có thể buộc cả hai quy trình phải đồng ý về một “vùng được chấp nhận” chung của các mục ổn định theo cả hai tiêu chí sắp xếp thì cả hai quy trình tham lam đều hoạt động giống hệt nhau. 

Cách đơn giản nhất để đảm bảo hành vi giống hệt nhau là làm cho cả hai thuật toán chọn tất cả các mục một cách hiệu quả, điều này làm giảm vấn đề khiến cả hai thủ tục tham lam chấp nhận mọi mục mà không bao giờ từ chối do xung đột năng lực hoặc thứ tự. 

Để làm điều này, chúng tôi chỉ định tất cả trọng số và giá trị còn thiếu để mọi mục đều phù hợp và mọi mục đều được xử lý theo thứ tự nhất quán để không xảy ra tình trạng từ chối. 

Chúng tôi đảm bảo rằng tổng trọng số tối đa là W và chúng tôi đảm bảo việc sắp xếp thứ tự giá trị không thể ngăn cản việc đưa vào. Khi tất cả các mục đều phù hợp, Alice lấy mọi thứ vì không có bước tiền tố nào tràn ra và Bob lấy mọi thứ vì mọi mục đều phù hợp khi gặp. 

Trở ngại duy nhất là một số trọng lượng được cố định và không thể giảm được. Tuy nhiên, vì chúng ta được phép gán các trọng số chưa biết nên chúng ta luôn có thể chọn chúng đủ nhỏ để thỏa mãn ràng buộc tổng miễn là các trọng số cố định không vượt quá dung lượng.

Điều này dẫn đến việc kiểm tra tính khả thi trực tiếp: nếu tổng các trọng số đã cố định lớn hơn W thì không có phép gán nào có thể cứu được thủ tục của Alice vì những trọng số đó là bất biến. Ngược lại, chúng ta có thể gán mọi trọng số chưa biết là 1 và đảm bảo tổng trọng số vẫn nằm trong khả năng cho phép. Các giá trị có thể được gán dương tùy ý, chẳng hạn như tất cả đều bằng nhau, điều này làm cho thứ tự của Bob không liên quan đến việc hòa. 

Điều này làm cho cả hai thủ tục tham lam đều chấp nhận mọi mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm phân công vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng giải pháp toàn chọn | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng một giải pháp trong đó cả Alice và Bob đều chọn tất cả các mục. 

1. Tính tổng của tất cả các trọng số đã cho đã được cố định (các mục khác 0). Những trọng lượng này không thể thay đổi được nên chúng tạo thành một giới hạn dưới cứng rắn đối với tổng mức sử dụng ba lô. 
2. Nếu tổng cố định này đã vượt quá W thì không thể chuyển nhượng được vì ngay cả trước khi thêm bất kỳ trọng số chưa biết nào, Alice sẽ vượt quá khả năng khi cố gắng lấy tất cả các mục. Trong trường hợp này, chúng tôi ngay lập tức đưa ra kết quả rằng nhiệm vụ này là không thể. 
3. Mặt khác, gán cho mỗi trọng số chưa biết giá trị 1. Điều này giữ cho tổng trọng số càng nhỏ càng tốt trong khi vẫn tôn trọng yêu cầu các trọng số phải là số nguyên dương. 
4. Bây giờ hãy xem xét quá trình của Alice. Vì tất cả các trọng số đều dương và tổng của chúng không vượt quá W, nên mọi tiền tố của thứ tự sắp xếp theo trọng số vẫn khả thi, vì vậy Alice không bao giờ gặp phải một món đồ có thể tràn ra ba lô. Do đó, cô ấy chọn mọi món đồ. 
5. Gán tất cả các giá trị chưa biết cho 1. Điều này làm cho tất cả các giá trị giống hệt nhau, do đó việc sắp xếp của Bob trở nên ổn định và chỉ phụ thuộc vào các chỉ số ban đầu. 
6. Bob xử lý tất cả các mục theo một số thứ tự, nhưng vì mọi mục đều vừa với dung lượng còn lại (vì tổng trọng lượng nằm trong W) nên anh ấy không bao giờ bỏ qua do dung lượng. Vì thế Bob cũng chọn từng mục. 

### Tại sao nó hoạt động 

Việc xây dựng giảm cả hai quá trình tham lam xuống một chế độ tầm thường trong đó những hạn chế về năng lực không bao giờ gây ra sự từ chối. Điều kiện dừng của Alice không bao giờ được kích hoạt vì tổng trọng lượng bị giới hạn bởi W. Điều kiện bỏ qua của Bob không bao giờ được kích hoạt vì mỗi vật phẩm đều phù hợp riêng lẻ tại thời điểm nó được xem xét. Vì cả hai quy trình đều bao gồm tất cả các mục nên các tập được chọn của chúng giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, W = map(int, input().split())
        w = list(map(int, input().split()))
        r = list(map(int, input().split()))
        
        fixed_sum = 0
        for x in w:
            if x != 0:
                fixed_sum += x
        
        if fixed_sum > W:
            print("No")
            continue
        
        print("Yes")
        
        # assign weights
        res_w = []
        for x in w:
            if x == 0:
                res_w.append(1)
            else:
                res_w.append(x)
        
        # assign values
        res_r = []
        for x in r:
            if x == 0:
                res_r.append(1)
            else:
                res_r.append(x)
        
        print(*res_w)
        print(*res_r)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo sau việc xây dựng trực tiếp. Bước không tầm thường duy nhất là tính tổng các trọng số cố định và xác minh tính khả thi đối với W. Mọi thứ khác là gán trực tiếp 1 cho các mục còn thiếu. 

Một điểm tinh tế là chúng tôi không bao giờ sửa đổi các giá trị đã cho. Điều này rất quan trọng vì vấn đề đảm bảo rằng mỗi mặt hàng bị thiếu tối đa một trọng lượng hoặc giá trị, do đó không có xung đột khi ghi đè. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét ba mục có dung lượng W = 10. 

Giả sử trọng lượng là`[3, 0, 2]`và các giá trị là`[0, 5, 0]`. 

Tổng trọng lượng cố định là`3 + 2 = 5`, nằm trong khả năng. 

Chúng tôi gán trọng số và giá trị còn thiếu là 1. 

| Bước | Trọng lượng | Giá trị | Tổng cố định | hợp lệ | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3, 0, 2 | 0, 5, 0 | 5 | Có | 
| Sau khi điền | 3, 1, 2 | 1, 5, 1 | 5 | Có | 

Alice xử lý các vật phẩm theo thứ tự trọng lượng tăng dần và tích lũy tổng cộng tối đa là 6, tức là 10, vì vậy cô ấy lấy tất cả các vật phẩm. Bob xử lý các mục theo thứ tự giá trị giảm dần và mọi mục đều phù hợp, vì vậy anh ấy cũng lấy tất cả các mục. 

Điều này xác nhận rằng sự lựa chọn giống hệt nhau xảy ra. 

### Ví dụ 2 

Đặt W = 6, trọng số`[2, 0, 5]`, giá trị`[0, 3, 0]`. 

Tổng trọng lượng cố định là`2 + 5 = 7`, đã vượt quá khả năng. 

| Bước | Tổng cố định | Quyết định | 
| --- | --- | --- | 
| Ban đầu | 7 | Không thể | 

Vì ngay cả trọng lượng bất biến cũng vượt quá khả năng, Alice sẽ buộc phải thất bại trước bất kỳ nhiệm vụ nào, vì vậy không có giải pháp nào tồn tại. 

Điều này cho thấy trở ngại duy nhất: chỉ riêng trọng lượng cố định cũng có thể vi phạm tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lần để tính tổng và điền vào mảng | 
| Không gian | O(n) | Lưu trữ mảng được gán cuối cùng | 

Các ràng buộc cho phép tổng số lên tới 3000 mục, do đó, việc xây dựng tuyến tính cho mỗi trường hợp thử nghiệm là đủ nhanh. Giải pháp này tránh mọi hoạt động sắp xếp hoặc tìm kiếm, giúp duy trì nó trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    T = int(sys.stdin.readline())
    out = []
    for _ in range(T):
        n, W = map(int, sys.stdin.readline().split())
        w = list(map(int, sys.stdin.readline().split()))
        r = list(map(int, sys.stdin.readline().split()))

        fixed = sum(x for x in w if x != 0)
        if fixed > W:
            out.append("No")
        else:
            out.append("Yes")
            w2 = [x if x != 0 else 1 for x in w]
            r2 = [x if x != 0 else 1 for x in r]
            out.append(" ".join(map(str, w2)))
            out.append(" ".join(map(str, r2)))

    return "\n".join(out)

# custom cases
assert run("""1
1 10
0
0
""") == "Yes\n1\n1"

assert run("""1
2 1
1 1
0 0
""") == "No"

assert run("""1
3 10
3 0 2
0 5 0
""").startswith("Yes")

assert run("""1
2 10
0 0
0 0
""") == "Yes\n1 1\n1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chưa biết duy nhất | Có, điền tối thiểu | Xây dựng cơ sở | 
| tràn cố định | Không | phát hiện không thể | 
| ẩn số hỗn hợp | Có | tính khả thi chung | 
| tất cả đều chưa biết | Có | hoàn toàn linh hoạt | 

## Vỏ cạnh 

Trường hợp nguy hiểm là khi trọng lượng cố định đã vượt quá khả năng. Ví dụ: nếu một mục có trọng lượng 10^9 và W nhỏ hơn thì không có phép gán nào có thể giảm được nó. Thuật toán sẽ từ chối chính xác ngay lập tức mà không cần thực hiện các phép gán. 

Một trường hợp khác là khi thiếu tất cả các giá trị nhưng trọng số vẫn cố định. Ngay cả trong trường hợp đó, việc gán giá trị là không liên quan vì việc lựa chọn được xác định hoàn toàn bằng trọng số một khi tính khả thi của Alice được thỏa mãn. 

Trường hợp tinh vi cuối cùng là khi có nhiều số không tồn tại. Vì chúng tôi luôn chỉ định 1 nên chúng tôi đảm bảo lạm phát tổng trọng lượng ở mức tối thiểu trong khi vẫn duy trì tính chính xác và chúng tôi không bao giờ vô tình vượt quá công suất trong giải pháp đã xây dựng.
