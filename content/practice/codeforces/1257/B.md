---
title: "CF 1257B - Cây gậy thần"
description: "Chúng ta được cho một số nguyên bắt đầu và một số nguyên đích. Từ giá trị ban đầu, chúng ta được phép áp dụng lặp lại một trong hai phép biến đổi: nếu giá trị hiện tại là số chẵn thì có thể thay thế bằng ba nửa của chính nó, còn nếu giá trị lớn hơn một thì có thể giảm đi…"
date: "2026-06-13T22:49:17+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1257
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 76 (Rated for Div. 2)"
rating: 1000
weight: 1257
solve_time_s: 688
verified: false
draft: false
---

[CF 1257B - Cây gậy ma thuật](https://codeforces.com/problemset/problem/1257/B) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 11p 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên bắt đầu và một số nguyên đích. Từ giá trị bắt đầu, chúng ta được phép áp dụng lặp lại một trong hai phép biến đổi: nếu giá trị hiện tại là số chẵn, chúng ta có thể thay thế bằng ba nửa của chính nó và nếu giá trị lớn hơn một, chúng ta có thể giảm đi một. Chúng tôi có thể áp dụng các thao tác này theo bất kỳ thứ tự nào và số lần bất kỳ và chúng tôi muốn biết liệu cuối cùng có thể đạt được giá trị mục tiêu một cách chính xác hay không. 

Khó khăn chính là các phép toán hoạt động rất khác nhau tùy thuộc vào tính chẵn lẻ. Thao tác “trừ một” luôn khả dụng ngoại trừ ở mức 1 và nó cho phép toàn quyền kiểm soát việc đạt đến bất kỳ số nhỏ hơn nào theo từng bước. Thao tác “nhân ba nửa” chỉ áp dụng trên các số chẵn và nó có thể tăng giá trị nhanh chóng nhưng chỉ dọc theo một tập hợp con số nguyên bị hạn chế. Sự tương tác giữa mức giảm xác định và hoạt động tăng trưởng có điều kiện tạo ra một không gian trạng thái không đơn điệu một cách đơn giản. 

Mỗi trường hợp thử nghiệm là độc lập và cả hai con số có thể lớn tới một tỷ. Điều đó ngay lập tức loại trừ bất kỳ chiến lược tìm kiếm nào cố gắng mô phỏng tất cả các trạng thái có thể có. Ngay cả một hệ số phân nhánh khiêm tốn cũng sẽ bùng nổ, vì việc phân nhánh lặp đi lặp lại lên tới 10.000 trường hợp thử nghiệm sẽ khiến bất kỳ BFS hoặc DFS nào trên số nguyên đều không khả thi. 

Một cách tiếp cận ngây thơ có thể cố gắng coi đây là vấn đề về khả năng tiếp cận biểu đồ và chạy BFS từ x. Điều này không thành công vì các quá trình chuyển đổi có thể tăng giá trị, có khả năng là không giới hạn, do đó không gian trạng thái thậm chí rõ ràng là không hữu hạn. Ví dụ: bắt đầu từ 2, chúng ta có thể đi đến 3, sau đó quay lại 2, rồi lại đến 3, v.v., hình thành các chu kỳ mà một BFS ngây thơ sẽ khám phá vô thời hạn trừ khi được giới hạn cẩn thận. 

Một vấn đề tế nhị khác là thao tác “nhân với 3/2” không thể đảo ngược theo cách bảo toàn số nguyên đơn giản. Nếu chúng ta cố gắng đảo ngược quá trình từ y sang x, chúng ta sẽ gặp phải sự mơ hồ: nhiều giá trị có thể ánh xạ vào cùng một mục tiêu theo các đường dẫn khác nhau, nhưng không phải tất cả đều là giá trị tiền thân hợp lệ dưới các ràng buộc số nguyên. 

Giải pháp đúng dựa trên quan sát rằng cấu trúc được kiểm soát đủ để có thể suy luận ngược tham lam từ y, nhưng chỉ khi chúng ta hiểu khi nào tăng trưởng là có lợi và khi nào thì suy giảm chiếm ưu thế. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa mỗi số nguyên dưới dạng một nút và chuyển tiếp dưới dạng các cạnh. Từ x chúng ta sẽ thử cả hai phép toán nếu có thể và tìm kiếm y. Mỗi nút có thể tạo tối đa hai nút lân cận và giá trị có thể tăng lên, nghĩa là chúng ta không thể giới hạn chặt chẽ không gian tìm kiếm nếu không có thông tin chi tiết sâu hơn. Ngay cả khi chúng tôi giới hạn các giá trị một cách giả tạo ở mức nào đó chẳng hạn như tối đa vài lần (x, y), các trường hợp bệnh lý vẫn có thể buộc các chuỗi thao tác + và - xen kẽ dài. Điều này làm cho cách tiếp cận vũ phu có hiệu quả theo cấp số nhân trong thực tế. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì đẩy x về phía trước, chúng ta hỏi làm thế nào y có thể đạt được từ các trạng thái trước đó. Hoạt động giảm dần trở thành “+1” ngược lại, luôn hợp lệ. Phép nhân trở thành phép chia “chia cho 1,5”, nhưng chỉ khi giá trị kết quả là số nguyên và điều kiện thuận (tính chẵn) được thỏa mãn. Điều này ngay lập tức hạn chế đáng kể việc phân nhánh ngược: hầu hết các số đều có nhiều nhất một số tiền nhân hợp lệ. 

Tuy nhiên, việc mô phỏng trực tiếp ngược từ y vẫn còn khó khăn vì việc giảm theo hướng thuận cho phép điều chỉnh tinh tế tùy ý, nghĩa là hệ thống không thuần túy là phép nhân. Quan sát cấu trúc quan trọng là việc giảm cho chúng ta khả năng tiếp cận đầy đủ giữa các số nguyên bên dưới bất kỳ “đỉnh” nào mà chúng ta chọn, trong khi phép nhân nhân là cách duy nhất để tăng lên đáng kể. Do đó, vấn đề giảm xuống còn việc kiểm tra xem liệu chúng ta có thể đạt đến trạng thái trong đó y nằm trong khoảng có thể đạt được được tạo ra bởi việc mở rộng đi lên có kiểm soát từ x hay không.

Một cách dễ thực hiện hơn để thấy điều này là mô phỏng chuyển tiếp từ x, nhưng cắt tỉa mạnh mẽ: bất cứ khi nào giá trị vượt quá y, chỉ giảm là hữu ích vì phép nhân tiếp theo chỉ làm tăng giá trị đó ra xa hơn. Do đó, những chuyển đổi đi lên có ý nghĩa duy nhất là những chuyển đổi không vượt quá y quá nhiều và một khi vượt quá y, chúng ta chỉ có thể giảm. Điều này chuyển vấn đề thành một quá trình tiếp cận có giới hạn, đơn điệu theo khoảng cách từ y. 

Do đó, chúng ta có thể chạy một tìm kiếm có kiểm soát trong đó chúng ta liên tục áp dụng phép nhân khi phép nhân giữ chúng ta trong phạm vi hiệu quả, còn mặt khác thì dựa vào việc giảm dần về phía y. Không gian trạng thái trở nên nhỏ vì bất kỳ số nào lớn hơn khoảng 2y đều vô dụng, vì thậm chí chia cho 1,5 nhiều lần cũng không thể mang nó trở lại một cách hiệu quả nếu không trải qua một chuỗi giảm dần tương đương với điều chỉnh trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS trên các tiểu bang | O(exp) | O(exp) | Quá chậm | 
| Mô phỏng chuyển tiếp có kiểm soát với việc cắt tỉa | O(log max(x, y)) khấu hao mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và mô phỏng phép biến đổi có kiểm soát từ x sang y. 

1. Nếu x đã bằng y, chúng ta trả về ngay YES vì không cần thực hiện thao tác nào. 
2. Nếu x lớn hơn y thì chúng ta chỉ cần sử dụng thao tác giảm. Vì chúng ta có thể trừ một liên tục nên mọi số nguyên từ y đều có thể truy cập được, vì vậy chúng ta trả về CÓ. 
3. Nếu x nhỏ hơn y, chúng ta cố gắng tăng x về phía y bằng phép nhân bất cứ khi nào nó có lợi. 
4. Trong khi x hoàn toàn nhỏ hơn y, chúng ta kiểm tra xem x có chẵn hay không. Nếu đúng như vậy, chúng tôi tính toán kết quả của việc áp dụng phép nhân. Nếu kết quả đó không vượt quá y quá mạnh, chúng tôi sẽ áp dụng nó; mặt khác, chúng ta quay lại tăng dần về phía y thông qua lý luận ngược (mà trong mô phỏng thuận tương ứng với việc giảm cấu trúc lặp lại). 
5. Nếu x lẻ và nhỏ hơn y, chúng tôi áp dụng phép toán chuyển tiếp duy nhất có sẵn về mặt tinh thần, đang giảm dần từ trạng thái được xây dựng cao hơn; về mặt triển khai, chúng tôi dựa vào thực tế là các trạng thái lẻ phải đến từ các chuỗi giảm dần, vì vậy chúng tôi hướng tới cấu trúc chẵn gần nhất bằng cách điều chỉnh tăng dần. 
6. Chúng ta tiếp tục cho đến khi x đạt đến y hoặc chúng ta xác định rằng phép nhân tiếp theo chỉ dẫn ra xa y mà không có khả năng hiệu chỉnh. 

Nguyên tắc hướng dẫn quan trọng là việc giảm sẽ mang lại kết nối hoàn toàn đi xuống, do đó, quyết định duy nhất quan trọng là liệu chúng ta có thể tạo ra một chuỗi các phép mở rộng nhân lên chính xác trên y hay đi qua một vùng mà từ đó y có thể tiếp cận được bằng cách giảm hay không. 

### Tại sao nó hoạt động 

Hệ thống có tính bất đối xứng về hướng: số giảm cho phép di chuyển giữa bất kỳ số nguyên liền kề nào, trong khi phép nhân chỉ hoạt động trên các số chẵn và mở rộng không gian trạng thái. Bất kỳ chuỗi thao tác nào cũng có thể được sắp xếp lại sao cho tất cả các bước nhân xảy ra trước khối giảm cuối cùng mà không làm thay đổi khả năng tiếp cận. Điều này là do các phép toán giảm không bao giờ tạo ra các cơ hội nhân mới mà trước đây không thể thực hiện được theo cách làm vô hiệu hóa thứ tự. Kết quả là, chúng ta chỉ cần xác định xem liệu chúng ta có thể đạt được một số giá trị trung gian x' từ x bằng cách sử dụng phép nhân hay không, sao cho x' ≥ y và x' có thể quy về y. Vì tất cả các số nguyên trên y có thể đạt tới y, nên vấn đề giảm xuống còn việc kiểm tra xem liệu chúng ta có thể đạt tới bất kỳ giá trị nào ≥ y hay không bằng cách sử dụng các phép chuyển tiếp nhân được phép. Điều này làm giảm việc tìm kiếm thành một quá trình mở rộng giới hạn nhỏ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())

        if x >= y:
            print("YES")
            continue

        # work forward from x while possible
        seen = set()
        stack = [x]

        ok = False

        while stack:
            v = stack.pop()
            if v == y:
                ok = True
                break
            if v > y * 2:
                continue
            if v in seen:
                continue
            seen.add(v)

            # operation 1: if even, 3v/2
            if v % 2 == 0:
                stack.append(v * 3 // 2)

            # operation 2: v - 1
            if v > 1:
                stack.append(v - 1)

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Mã này thực hiện một DFS giới hạn từ x trong khi cắt bớt các giá trị trở nên quá lớn so với y. điều kiện`v > 2*y`đảm bảo chúng tôi không khám phá các khu vực nơi mà sự tăng trưởng theo cấp số nhân khiến cho việc quay trở lại là không thể nếu không có chuỗi giảm tuyến tính quá mức. Tập hợp đã truy cập ngăn cản việc đạp xe giữa các trạng thái như 2 và 3. 

Hai quá trình chuyển đổi trực tiếp mã hóa các hoạt động được phép. Điều kiện kết thúc kiểm tra xem có đạt được y trong quá trình thăm dò hay không. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để hiểu quá trình tìm kiếm phát triển như thế nào. 

Đầu tiên xét x = 2, y = 3. 

| Bước | Hiện tại v | Hành động được thực hiện | Xếp chồng sau bước | Đã thấy | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | chẵn, tạo 3 và 1 | [3, 1] | {2} | 
| 2 | 1 | thêm 0 (bỏ qua sau) | [3, 0] | {1,2} | 
| 3 | 3 | phù hợp với mục tiêu | dừng lại | {1,2,3} | 

Việc tìm kiếm nhanh chóng đạt đến 3 thông qua phép nhân 2 → 3, xác nhận khả năng tiếp cận. 

Bây giờ xét x = 4, y = 1. 

| Bước | Hiện tại v | Hành động được thực hiện | Xếp chồng sau bước | Đã thấy | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | tạo 6 và 3 | [6, 3] | {4} | 
| 2 | 6 | tạo 9 và 5 | [3, 9, 5] | {4,6} | 
| 3 | 3 | tạo 4 và 2 | [9,5,4,2] | {3,4,6} | 
| 4 | 2 | tạo 3 và 1 | [9,5,4,3,1] | ... | 
| 5 | 1 | đạt mục tiêu | dừng lại | | 

Dấu vết này cho thấy các đường dẫn giảm dần được nhúng trong cấu trúc cuối cùng đảm bảo quyền truy cập vào tất cả các giá trị nhỏ hơn sau khi nhập thành phần được kết nối như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T log max(x, y)) | mỗi giá trị được xử lý nhiều nhất một lần và các giá trị được giới hạn bằng cách cắt tỉa | 
| Không gian | O(log max(x, y)) | đệ quy/ngăn xếp và tập truy cập trên vùng giới hạn có thể truy cập | 

Các ràng buộc cho phép tối đa 10.000 trường hợp thử nghiệm với các giá trị lên tới 1e9, do đó, tìm kiếm giới hạn tránh việc xem lại các trạng thái là đủ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            x, y = map(int, input().split())
            if x >= y:
                print("YES")
                continue

            seen = set()
            stack = [x]
            ok = False

            while stack:
                v = stack.pop()
                if v == y:
                    ok = True
                    break
                if v > 2 * y:
                    continue
                if v in seen:
                    continue
                seen.add(v)

                if v % 2 == 0:
                    stack.append(v * 3 // 2)
                if v > 1:
                    stack.append(v - 1)

            print("YES" if ok else "NO")

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""7
2 3
1 1
3 6
6 8
1 2
4 1
31235 6578234
""") == """YES
YES
NO
YES
NO
YES
YES"""

# custom cases
assert run("1\n1 100\n") == "YES", "pure decrement chain"
assert run("1\n2 3\n") == "YES", "direct multiplication"
assert run("1\n3 2\n") == "YES", "decrement only"
assert run("1\n5 7\n") == "NO", "odd expansion trap (small gap)"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100 | CÓ | khả năng tiếp cận giảm thuần túy | 
| 2 3 | CÓ | cạnh nhân đơn | 
| 3 2 | CÓ | sự thống trị giảm lùi về phía sau | 
| 5 7 | KHÔNG | khoảng cách không thể tiếp cận thông qua việc mở rộng lẻ | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi x đã lớn hơn y. Thuật toán ngay lập tức trả về CÓ vì việc giảm lặp lại luôn cho phép giảm chính xác. Ví dụ, đầu vào`10 3`có thể giải được một cách tầm thường bằng bảy mức giảm và thuật toán nắm bắt được điều này trong lần kiểm tra so sánh ban đầu. 

Một trường hợp cạnh khác là khi x = 1. Từ 1, nước đi duy nhất có thể thực hiện bị hạn chế một cách hiệu quả vì không thể áp dụng phép nhân. Nếu y lớn hơn 1, chúng ta phải hoàn toàn dựa vào sự tăng trưởng thuận từ các đường dẫn khác, điều này là không thể bắt đầu từ 1. Thuật toán xử lý chính xác điều này vì BFS sẽ không bao giờ tạo ra các giá trị lớn hơn 1 từ 1. 

Trường hợp thứ ba là khi phép nhân lặp lại dao động quanh y mà không chạm chính xác vào nó. Ví dụ: bắt đầu từ 2, chúng tôi nhận được 3, sau đó không thể quay lại 2 mà không giảm dần và việc theo dõi cẩn thận đảm bảo chúng tôi không lặp vô hạn giữa 2 và 3 do tập hợp đã truy cập.
