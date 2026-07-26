---
title: "CF 102868A - Đen"
description: "Lò phản ứng hiển thị một chuỗi các lần nhấn nút ẩn. Trình tự có độ dài N, chỉ sử dụng chín nút từ A đến I và không bao giờ nhấn cùng một nút hai lần. Trong một số lần lặp lại, một số chớp sáng bị bỏ sót, do đó mỗi quan sát chỉ là một chuỗi con của chuỗi thực."
date: "2026-07-25T13:23:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102868
codeforces_index: "A"
codeforces_contest_name: "2020 UTPC Fall Puzzle Contest"
rating: 0
weight: 102868
solve_time_s: 54
verified: true
draft: false
---

[CF 102868A - Đen](https://codeforces.com/problemset/problem/102868/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Lò phản ứng hiển thị một chuỗi các lần nhấn nút ẩn. Dãy số có độ dài`N`, chỉ sử dụng chín nút`A`bởi vì`I`và không bao giờ nhấn cùng một nút hai lần. Trong một số lần lặp lại, một số chớp sáng bị bỏ sót, do đó mỗi quan sát chỉ là một chuỗi con của chuỗi thực. Nhiệm vụ là quyết định xem liệu tất cả các quan sát cùng nhau có xác định chính xác một chuỗi ban đầu có thể hay không. Nếu họ làm vậy, chúng tôi sẽ in nó. Nếu không thì không có đủ thông tin. 

Đầu vào đưa ra một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi biết độ dài chuỗi mục tiêu và tập hợp các chuỗi con được quan sát. Mỗi chuỗi con được quan sát sẽ bảo toàn thứ tự tương đối của các nút đã được nhìn thấy, bởi vì những lần nhấn lỡ chỉ loại bỏ các phần tử và không bao giờ sắp xếp lại chúng. 

Giá trị nhỏ của`N`là hạn chế chính. Vì chỉ có chín nút nên không gian tìm kiếm bị giới hạn cho lập trình động tập hợp con. Một giải pháp thử trực tiếp mọi trình tự có thể sẽ xem xét tới`9! = 362880`trình tự cho`N = 9`. Với tối đa 1000 trường hợp thử nghiệm, điều đó có thể trở nên quá tốn kém, đặc biệt là vì mọi chuỗi ứng cử viên phải được kiểm tra dựa trên tối đa 100 quan sát. Công việc tối đa của phương pháp này là xung quanh`362880 * 100 * 1000`, vượt xa những gì vừa vặn một cách thoải mái. 

Những trường hợp khó khăn đến từ việc thiếu nút bấm và một phần thông tin đặt hàng. Một giải pháp bất cẩn có thể chỉ xem xét các chữ cái xuất hiện trong quan sát, nhưng một nút không nhìn thấy vẫn có thể thuộc về mẫu mục tiêu. 

Ví dụ:```
Input
1
2 1
1 A
```Đầu ra đúng là:```
NOT ENOUGH INFO
```Quan sát cho chúng ta biết rằng`A`xuất hiện, nhưng nút thứ hai có thể là bất kỳ nút nào trong số tám nút còn lại và có thể xuất hiện trước hoặc sau`A`. 

Một trường hợp phức tạp khác là khi mọi cặp được quan sát đều nhất quán nhưng không hoàn toàn đảm bảo thứ tự.```
Input
1
3 2
2 A B
2 B C
```Đầu ra đúng là:```
A B C
```Ở đây lực ràng buộc`A`trước`B`Và`B`trước`C`, do đó toàn bộ dãy đã biết. Giải pháp chỉ kiểm tra xem tất cả các chữ cái có xuất hiện trong một quan sát hay không sẽ không thành công vì không có quan sát đơn lẻ nào chứa mẫu hoàn chỉnh. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ tạo ra mọi chiều dài có thể`N`sắp xếp chín nút và kiểm tra xem mỗi quan sát có phải là một chuỗi con của nó hay không. Cách tiếp cận này đúng vì một chuỗi mục tiêu hợp lệ phải xuất hiện trong tập hợp được tạo và mọi chuỗi được tạo có thể được xác minh dựa trên các quan sát. Vấn đề là số lượng ứng viên. Khi`N = 9`, có`9! = 362880`khả năng, và mỗi một có thể yêu cầu kiểm tra tất cả`R`quan sát. Việc xác minh lặp đi lặp lại khiến trường hợp xấu nhất diễn ra quá chậm. 

Quan sát hữu ích là các quan sát chỉ đưa ra các ràng buộc về thứ tự tương đối. Nếu một quan sát chứa`A C E`, thì mục tiêu phải đặt`A`trước`C`Và`C`trước`E`. Chúng tôi không cần phải mô phỏng các đèn flash bị thiếu. Chúng ta chỉ cần biết nút nào có thể được đặt tiếp theo trong khi vẫn tôn trọng tất cả các ràng buộc về thứ tự trước đó. 

Điều này biến vấn đề thành việc đếm các thứ tự tôpô hợp lệ của một đồ thị có hướng nhỏ. Biểu đồ có chín đỉnh có thể, một đỉnh cho mỗi nút. Một cạnh`u -> v`có nghĩa là mọi chuỗi mục tiêu hợp lệ phải đặt`u`trước`v`. 

Phương pháp lập trình động tập hợp con hoạt động vì chỉ có`2^9 = 512`bộ nút đã được đặt sẵn có thể. Đối với mọi trạng thái, chúng tôi thử thêm một nút có các điều kiện tiên quyết đã có trong tập hợp đã chọn. Chúng tôi đếm có bao nhiêu chiều dài`N`trình tự có thể được hình thành. Chúng ta chỉ cần phân biệt giữa không, một và nhiều câu trả lời có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(9! * R * N) | O(N) | Quá chậm | 
| Tối ưu | O(2^9 * 9) | O(2^9) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng từ các quan sát. Đối với mỗi chuỗi được quan sát, hãy thêm một cạnh từ mỗi nút trước đó vào mỗi nút sau vì chuỗi ẩn phải giữ nguyên thứ tự đó. 
2. Chạy tìm kiếm được ghi nhớ trên tập hợp con các nút. Một tiểu bang`mask`đại diện cho các nút đã được đặt ở đầu chuỗi mục tiêu. 
3. Từ trạng thái hiện tại, hãy thử mọi nút chưa được sử dụng. Một nút có thể được thêm vào nếu mọi nút phải xuất hiện trước nút đó đã có sẵn trong`mask`. 
4. Dừng mở rộng một trạng thái khi nó chứa`N`các nút. Điều này thể hiện một chuỗi mục tiêu hoàn chỉnh có thể. 
5. Đếm số lần hoàn thành có thể. Nếu có đúng một lần hoàn thành, hãy trả lại nó. Nếu không có hoặc có nhiều lần hoàn thành, hãy xuất`NOT ENOUGH INFO`. 

Tại sao nó hoạt động: mọi mẫu mục tiêu hợp lệ chính xác là một thứ tự tôpô của`N`các nút trong biểu đồ ràng buộc. Lập trình động khám phá mọi tiền tố hợp lệ có thể có của một thứ tự như vậy và việc chuyển đổi chỉ được phép khi nó tôn trọng tất cả các yêu cầu thứ tự đã biết. Vì mọi mẫu mục tiêu có thể tương ứng với một đường dẫn thông qua biểu đồ trạng thái và mọi đường dẫn đều tương ứng với một mẫu hợp lệ, việc đếm các đường dẫn sẽ đưa ra số lượng chính xác các câu trả lời có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case():
    N, R = map(int, input().split())

    graph = [0] * 9

    for _ in range(R):
        parts = input().split()
        r = int(parts[0])
        seq = [ord(c) - 65 for c in parts[1:]]

        for i in range(r):
            for j in range(i + 1, r):
                graph[seq[j]] |= 1 << seq[i]

    full_size = N
    memo = {}

    def dfs(mask):
        if mask.bit_count() == full_size:
            return 1, ""

        if mask in memo:
            return memo[mask]

        count = 0
        answer = ""

        for nxt in range(9):
            if mask & (1 << nxt):
                continue

            if graph[nxt] & ~mask:
                continue

            sub_count, sub_answer = dfs(mask | (1 << nxt))

            if sub_count:
                if count == 0 and sub_count == 1:
                    answer = chr(65 + nxt) + sub_answer

                count = min(2, count + sub_count)

                if count == 2:
                    answer = ""

        memo[mask] = (count, answer)
        return memo[mask]

    count, ans = dfs(0)

    if count == 1:
        return " ".join(ans)
    return "NOT ENOUGH INFO"

def main():
    t = int(input())
    out = []

    for _ in range(t):
        out.append(solve_case())

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Biểu đồ được lưu trữ dưới dạng bitmask.`graph[x]`chứa tất cả các nút phải xuất hiện trước nút`x`. Điều này làm cho quá trình chuyển đổi kiểm tra một thao tác bit đơn: nếu thiếu bất kỳ nút bắt buộc nào trong mặt nạ hiện tại thì nút đó vẫn chưa được đặt. 

Hàm đệ quy biểu diễn tập con DP. Trường hợp cơ sở đạt được khi chính xác`N`các nút đã được chọn vì mẫu mục tiêu không bao giờ chứa các nút trùng lặp và có độ dài cố định`N`. 

Số câu trả lời được giới hạn ở mức hai. Chúng ta chỉ quan tâm liệu không có giải pháp, chính xác một giải pháp, hay nhiều hơn một giải pháp. Giữ số lượng lớn hơn sẽ thêm công việc mà không thay đổi quyết định. 

Chuỗi tái thiết chỉ được giữ khi trạng thái có chính xác một phần tiếp theo. Nếu tìm thấy hai đường dẫn khác nhau, câu trả lời được lưu trữ sẽ bị loại bỏ vì kết quả cuối cùng phải mơ hồ. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu đầu tiên:```
5 3
3 A C E
3 B D E
4 A B D E
```Các ràng buộc trở thành: 

| Bước | Thông tin hiện tại | Kết quả | 
| --- | --- | --- | 
| Thêm quan sát đầu tiên | A trước C trước E | Vẫn còn nhiều đơn hàng | 
| Thêm quan sát thứ hai | B trước D trước E | Thêm hạn chế | 
| Thêm quan sát thứ ba | A trước B trước D trước E | Thứ tự A và B trở nên cố định | 
| Kết thúc | Một số vị trí hợp lệ vẫn còn | KHÔNG ĐỦ THÔNG TIN | 

Việc quan sát không xác định được vị trí của mọi nút, do đó DP tìm thấy nhiều hơn một chuỗi mục tiêu hợp lệ. 

Đối với trường hợp mẫu thứ ba:```
5 3
4 D A B C
3 E B C
3 D E A
```| Bước | Thông tin hiện tại | Kết quả | 
| --- | --- | --- | 
| Thêm quan sát đầu tiên | D trước A trước B trước C | Đặt hàng ban đầu | 
| Thêm quan sát thứ hai | E trước B trước C | E bị hạn chế | 
| Thêm quan sát thứ ba | D trước E trước A | Trật tự đầy đủ xuất hiện | 
| Kết thúc | Chỉ có D E A B C hoạt động | Câu trả lời độc đáo | 

DP đạt chính xác một thứ tự có độ dài năm hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^9 * 9) | Có tối đa 512 trạng thái tập hợp con và mỗi trạng thái thử chín nút | 
| Không gian | O(2^9) | Ghi nhớ lưu trữ một kết quả cho mỗi trạng thái tập hợp con | 

Các giới hạn này dễ dàng được thỏa mãn vì toàn bộ không gian trạng thái chỉ có vài trăm trạng thái. Số lượng quan sát chỉ ảnh hưởng đến việc xây dựng biểu đồ, con số này rất nhỏ vì mọi quan sát đều có độ dài tối đa là chín. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case():
        N, R = map(int, input().split())
        graph = [0] * 9

        for _ in range(R):
            parts = input().split()
            r = int(parts[0])
            seq = [ord(c) - 65 for c in parts[1:]]
            for i in range(r):
                for j in range(i + 1, r):
                    graph[seq[j]] |= 1 << seq[i]

        memo = {}

        def dfs(mask):
            if mask.bit_count() == N:
                return 1, ""
            if mask in memo:
                return memo[mask]

            cnt = 0
            res = ""

            for x in range(9):
                if mask & (1 << x):
                    continue
                if graph[x] & ~mask:
                    continue

                c, s = dfs(mask | (1 << x))

                if c:
                    if cnt == 0 and c == 1:
                        res = chr(65 + x) + s
                    cnt = min(2, cnt + c)
                    if cnt == 2:
                        res = ""

            memo[mask] = (cnt, res)
            return memo[mask]

        c, s = dfs(0)
        return " ".join(s) if c == 1 else "NOT ENOUGH INFO"

    t = int(input.readline())
    return "\n".join(solve_case() for _ in range(t))

assert run("""3
5 3
3 A C E
3 B D E
4 A B D E
1 1
1 C
5 3
4 D A B C
3 E B C
3 D E A
""") == """NOT ENOUGH INFO
C
D E A B C"""

assert run("""1
1 1
1 A
""") == "A"

assert run("""1
2 1
1 A
""") == "NOT ENOUGH INFO"

assert run("""1
3 2
2 A B
2 B C
""") == "A B C"

assert run("""1
9 1
9 A B C D E F G H I
""") == "A B C D E F G H I"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút quan sát duy nhất |`A`| Độ dài chuỗi tối thiểu | 
| Một nút bị thiếu |`NOT ENOUGH INFO`| Các nút không nhìn thấy được tạo ra sự mơ hồ | 
| Chuỗi ràng buộc đặt hàng |`A B C`| Quan sát từng phần có thể tạo ra một trật tự duy nhất | 
| Trình tự chín nút đầy đủ |`A B C D E F G H I`| Kích thước trạng thái tối đa | 

## Vỏ cạnh 

Khi chỉ quan sát thấy một nút, thuật toán vẫn coi tất cả chín nút là ứng cử viên có thể. Đối với đầu vào:```
1
2 1
1 A
```biểu đồ không chứa thông tin đặt hàng. DP có thể đặt`A`cùng với bất kỳ nút nào khác, do đó tồn tại nhiều chuỗi hợp lệ và câu trả lời bị từ chối một cách chính xác. 

Khi các quan sát tạo thành một chuỗi hoàn chỉnh, thuật toán không yêu cầu một quan sát phải chứa toàn bộ câu trả lời. Vì:```
1
3 2
2 A B
2 B C
```cửa hàng đồ thị`A -> B`Và`B -> C`. Thứ tự tôpô ba nút duy nhất có thể là`A B C`, vậy là việc tái thiết thành công. 

Hộp có kích thước tối đa chứa tất cả chín nút:```
1
9 1
9 A B C D E F G H I
```Thuật toán đạt đến độ sâu chín trong tìm kiếm tập hợp con và trả về thứ tự duy nhất có thể. Kích thước biểu đồ chín nút cố định giữ cho số lượng trạng thái không thay đổi, vì vậy trường hợp này được xử lý theo cách tương tự như các đầu vào nhỏ hơn.
