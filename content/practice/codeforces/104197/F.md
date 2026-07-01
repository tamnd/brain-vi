---
title: "CF 104197F - Đồ thị 3 màu F***"
description: "Chúng ta được cho một đồ thị vô hướng liên thông. Các đỉnh về mặt khái niệm đã được chia thành hai nhóm theo chỉ số, nhưng sự phân chia đó chỉ được sử dụng như một thủ thuật tô màu ban đầu: các đỉnh trong nhóm đầu tiên có thể được tô màu khác với nhóm thứ hai để đồ thị ban đầu…"
date: "2026-07-02T00:10:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104197
codeforces_index: "F"
codeforces_contest_name: "Anton Trygub Contest 1 (The 1st Universal Cup, Stage 4: Ukraine)"
rating: 0
weight: 104197
solve_time_s: 49
verified: true
draft: false
---

[CF 104197F - F*** Đồ thị 3 màu](https://codeforces.com/problemset/problem/104197/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông. Các đỉnh về mặt khái niệm đã được chia thành hai nhóm theo chỉ số, nhưng sự phân chia đó chỉ được sử dụng như một thủ thuật tô màu ban đầu: các đỉnh trong nhóm đầu tiên có thể được tô màu khác với nhóm thứ hai để đồ thị ban đầu có thể tô được 2 màu, do đó cũng có thể tô được 3 màu. 

Nhiệm vụ không phải là tô màu trực tiếp đồ thị đã cho. Thay vào đó, chúng ta được phép thêm các cạnh và chúng ta muốn biết số cạnh nhỏ nhất được thêm vào có thể khiến đồ thị không thể tô được 3 màu. Điểm mấu chốt là chúng tôi không sửa đổi các cạnh hiện có, chỉ chèn các cạnh mới và chúng tôi đang hỏi xem chúng tôi có thể phá hủy khả năng 3 màu nhanh đến mức nào. 

Đầu ra hóa ra cực kỳ nhỏ: nó luôn là 2 hoặc 3. Vì vậy, toàn bộ vấn đề giảm xuống việc phát hiện xem liệu chúng ta có thể tạo ra sự mâu thuẫn với 3 màu bằng cách sử dụng hai cạnh được thêm vào hay liệu chúng ta có cần ít nhất ba cạnh hay không. 

Khó khăn không rõ ràng là việc thêm một cạnh luôn có thể được xử lý bằng cách đưa ra một màu mới cho một điểm cuối, vì vậy một cạnh không bao giờ là đủ. Câu hỏi thực sự là cấu trúc nào cho phép hai cạnh tạo ra vật cản K4. 

Trường hợp cạnh tinh vi phát sinh khi biểu đồ chứa cấu trúc gần như tạo thành nhóm 4 cạnh sau khi thêm hai cạnh. Ví dụ: nếu có bốn đỉnh u1, v1, u2, v2 sao cho ba trong số bốn cạnh chéo có thể đã tồn tại thì việc cộng hai cạnh còn thiếu sẽ hoàn thành K4. Trong tình huống đó, việc tô 3 màu trở nên bất khả thi. Một ý tưởng ngây thơ chỉ kiểm tra mật độ cục bộ của các cạnh không thành công vì vật cản phụ thuộc vào tương tác 4 đỉnh rất cụ thể, không chỉ mức độ hoặc sự hiện diện của tam giác. 

Một trường hợp thất bại khác xuất hiện nếu người ta chỉ cố gắng suy luận về các hình tam giác. Một hình tam giác có thể tô được 3 màu nên không giúp ích gì, nhưng cấu trúc 4 chu kỳ ẩn có thể ám chỉ sự tồn tại của một cụm gần sau khi thêm các cạnh. 

Các ràng buộc ngụ ý rằng chúng ta cần lý luận đại khái là O(n^2) hoặc O(nm). Bất cứ điều gì dạng lập phương hoặc liên quan đến việc kiểm tra trực tiếp tất cả bốn đỉnh đều quá chậm vì số lượng tứ đỉnh là O(n^4), điều này hoàn toàn không khả thi đối với các giới hạn điển hình. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các cặp cạnh mà chúng ta có thể thêm vào và kiểm tra xem biểu đồ kết quả có thể tô được 3 màu hay không. Mỗi thử nghiệm như vậy yêu cầu kiểm tra màu đồ thị đầy đủ hoặc tìm kiếm quay lui và có thể có O(n^2) cạnh được thêm vào, dẫn đến kết quả ít nhất là O(n^2 · (n + m)), con số này quá lớn. 

Một quan điểm mang tính cấu trúc hơn là đặt câu hỏi cấu hình nào của các cạnh được thêm vào có thể ngay lập tức phá vỡ khả năng 3 màu. Với một cạnh được thêm vào, không có gì nghiêm trọng xảy ra vì chúng ta luôn có thể gán màu thứ ba mới cho một điểm cuối. Vì vậy, chúng tôi kiểm tra hai cạnh được thêm vào. 

Nếu hai cạnh được thêm có chung một đỉnh, chúng ta có thể tách đỉnh đó thành màu thứ ba, như vậy trường hợp đó là an toàn. Tình huống nguy hiểm duy nhất là khi hai cạnh được thêm vào rời nhau và nối bốn đỉnh phân biệt. Bây giờ vấn đề trở thành: bốn đỉnh đó có thể bị ép thành K4 sau khi thêm hai cạnh bị thiếu không? 

Điều này biến bài toán toàn cục thành bài toán mẫu cấm cục bộ trên bốn đỉnh. Đồ thị trở nên không thể tô màu 3 với hai cạnh được thêm chính xác khi tồn tại một tập hợp bốn đỉnh đã tạo thành một cấu trúc trong đó có ít nhất ba trong số bốn cạnh chéo có thể tồn tại giữa hai cặp. Điều này tương đương với sự tồn tại của cấu trúc 4 chu kỳ có thể được hoàn thành thành một cụm. 

Nếu không có cấu hình như vậy thì hai cạnh không bao giờ là đủ, nhưng ba cạnh luôn đủ vì chúng ta luôn có thể hoàn thành K4 trên một số bộ tứ đã chọn bằng cách thêm tất cả các cạnh bị thiếu.

Vì vậy, nhiệm vụ giảm xuống còn việc phát hiện xem biểu đồ có chứa cấu trúc 4 chu kỳ theo nghĩa này hay không. Một cách tiêu chuẩn để làm điều này là kiểm tra xem có đỉnh nào nằm trên một chu trình có độ dài bằng bốn hay không. Nếu chúng ta tìm thấy một chu trình như vậy thì nó tương ứng với hai đỉnh có chung ít nhất hai lân cận chung, đó chính xác là điều kiện cho phép cấu hình nguy hiểm. 

Chúng tôi so sánh các phương pháp dưới đây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hãy thử tất cả các cặp cạnh đã thêm + tính toán lại màu | O(n^2 · (n + m)) | O(n + m) | Quá chậm | 
| Kiểm tra tất cả các bộ tứ | O(n^4) | O(1) | Quá chậm | 
| Phát hiện 4 chu kỳ qua các nút giao thông lân cận | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi xử lý từng đỉnh v và coi nó như một phần tiềm năng của cấu trúc 4 chu kỳ. Mục đích là để phát hiện xem v có tham gia vào chu kỳ có độ dài bốn hay không. 
2. Đối với đỉnh v cố định, ta xét tất cả các đỉnh lân cận của nó u1, u2, ..., uk. Bất kỳ 4 chu trình nào liên quan đến v đều phải đi qua hai lân cận phân biệt của v, ví dụ u và u', rồi quay trở lại v qua một đỉnh khác. 
3. Chúng ta quét tất cả các lân cận của mỗi lân cận u của v và đếm xem mỗi đỉnh xuất hiện bao nhiêu lần trong số các lân cận thứ cấp này, bỏ qua chính v. Nếu một đỉnh x nào đó xuất hiện ít nhất hai lần trong quá trình này thì sẽ có hai đường đi v-u-x và v-u'-x riêng biệt, tạo thành một chu trình 4. 
4. Chúng tôi thực hiện điều này bằng cách đánh dấu các nút đã truy cập cho mỗi v bằng cách sử dụng mảng bộ đếm tạm thời hoặc kỹ thuật dấu thời gian, đảm bảo rằng chúng tôi phát hiện lượt truy cập lặp lại trong O(1) cho mỗi lần kiểm tra. 
5. Nếu bất kỳ đỉnh v nào tham gia vào cấu trúc như vậy, chúng ta kết luận ngay rằng câu trả lời là 2. Nếu không có cấu trúc nào như vậy tồn tại ở bất kỳ đâu trong biểu đồ thì câu trả lời là 3. 

### Tại sao nó hoạt động 

Một chu trình 4 tồn tại chính xác khi có hai lân cận riêng biệt của v có chung một lân cận khác ngoài v. Hàng xóm chung đó tạo ra hai đường dẫn có độ dài-2 khác nhau giữa những lân cận đó, đóng một chu trình có độ dài bốn. Đây chính xác là cấu hình cho phép bốn đỉnh được kết nối gần như đầy đủ sau khi chỉ thêm hai cạnh, cho phép hoàn thành K4 và phá vỡ khả năng 3 màu. Nếu không có sự chồng chéo như vậy tồn tại ở bất kỳ đâu thì không có cặp cạnh nào được thêm vào có thể tạo ra đồ thị con 4 đỉnh dày đặc cần thiết, do đó cần có ít nhất ba cạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    seen = [0] * n
    timer = 1

    for v in range(n):
        timer += 1
        mark_list = []

        for u in g[v]:
            for x in g[u]:
                if x == v:
                    continue
                if seen[x] == timer:
                    print(2)
                    return
                if seen[x] == timer - 1:
                    continue
                seen[x] = timer
                mark_list.append(x)

        # reset implicitly by timer change

    print(3)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc quét các lân cận thứ hai của mỗi đỉnh. các`seen`mảng được sử dụng làm điểm đánh dấu thời gian nên chúng ta không cần xóa nó cho mọi đỉnh. Khi gặp một nút hai lần trong cùng một lần lặp, chúng tôi sẽ ngay lập tức phát hiện cấu trúc 4 chu kỳ bị cấm và trả về 2. 

Điều tinh tế quan trọng là tránh đặt lại bậc hai. Thay vì xóa một mảng cho mọi đỉnh, chúng ta sử dụng một phép thay đổi`timer`để những nhãn hiệu cũ tự động trở nên không còn phù hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét 4 chu kỳ đơn giản: 1-2-3-4-1. 

| v | hàng xóm | quét hàng xóm thứ hai | tìm thấy trùng lặp | 
| --- | --- | --- | --- | 
| 1 | 2, 4 | từ 2 và 4 chúng ta đạt 3 hai lần | vâng | 

Khi xử lý đỉnh 1, cả đỉnh 2 và 4 đều trỏ đến 3 thông qua danh sách kề của chúng, do đó đạt đến đỉnh 3 hai lần. Điều này xác nhận cấu trúc 4 chu kỳ và câu trả lời là 2. 

Dấu vết này cho thấy thuật toán xác định chính xác các đường dẫn hai bước chồng chéo, đây chính xác là tín hiệu cấu trúc mà chúng tôi dựa vào. 

### Ví dụ 2 

Một cây trên bốn nút: 1-2-3-4. 

| v | hàng xóm | quét hàng xóm thứ hai | tìm thấy trùng lặp | 
| --- | --- | --- | --- | 
| 2 | 1, 3 | 1 đạt không có gì mới, 3 đạt 4 | không | 

Không có đỉnh nào xuất hiện hai lần trong bất kỳ lần quét lân cận thứ hai nào, do đó không tồn tại 4 chu kỳ. Đồ thị vẫn đủ thưa để hai cạnh được thêm vào không thể tạo ra K4, vì vậy câu trả lời là 3. 

Điều này xác nhận rằng việc không có xung đột chung với láng giềng thứ hai tương ứng với sự an toàn dưới sự bổ sung hai cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi đỉnh quét danh sách kề của các lân cận của nó và tổng số mở rộng lân cận được giới hạn bởi tổng bình phương, là O(n^2) trong các trường hợp dày đặc | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng đánh dấu phụ | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc điển hình cho n lên tới khoảng 2e5 trong các biểu đồ thưa thớt hoặc các biểu đồ dày đặc nhỏ hơn, vì thuật toán tránh mọi phép liệt kê bậc cao hơn và chỉ thực hiện các mở rộng lân cận có kiểm soát. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# small 4-cycle -> 2
assert run("4 4\n1 2\n2 3\n3 4\n4 1\n") == "2"

# tree -> 3
assert run("4 3\n1 2\n2 3\n3 4\n") == "3"

# star (no cycle) -> 3
assert run("5 4\n1 2\n1 3\n1 4\n1 5\n") == "3"

# triangle with tail -> 3
assert run("4 4\n1 2\n2 3\n3 1\n3 4\n") == "3"

# square with diagonal (still contains C4 structure) -> 2
assert run("4 5\n1 2\n2 3\n3 4\n4 1\n1 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 chu kỳ | 2 | phát hiện cấu trúc bị cấm tối thiểu | 
| đồ thị chuỗi | 3 | không có chu kỳ bị phát hiện sai | 
| đồ thị sao | 3 | trung tâm cấp cao không kích hoạt dương tính giả | 
| tam giác + lá | 3 | bỏ qua các hình tam giác không liên quan | 
| vuông + hợp âm | 2 | sự chắc chắn dưới các cạnh phụ | 

## Vỏ cạnh 

Trường hợp cạnh điển hình là khi đồ thị gần như có 4 chu kỳ nhưng có thêm dây cung. Ví dụ: một hình vuông có một đường chéo vẫn chứa hai đường dẫn có độ dài 2 riêng biệt cần thiết giữa các đỉnh đối diện. Khi xử lý một trong các đỉnh đó, thuật toán vẫn thấy xung đột lân cận thứ hai lặp đi lặp lại, vì cả hai lân cận trung gian đều trỏ đến một điểm cuối chung. Thuật toán trả về đúng 2. 

Một trường hợp cạnh khác là cây hoặc bất kỳ đồ thị tuần hoàn nào. Vì không có chu trình nào cả, mỗi phép mở rộng lân cận thứ hai đều tạo ra các tập hợp rời rạc, do đó không có đỉnh nào được tính hai lần. Thuật toán quét tất cả các đỉnh và không bao giờ kích hoạt, trả về 3 như mong đợi. 

Trường hợp tinh vi cuối cùng là một đồ thị dày đặc có độ lớn. Ngay cả khi có nhiều hàng xóm tồn tại, việc trùng lặp chỉ có vấn đề khi hai hàng xóm khác biệt có chung một hàng xóm. Phương pháp dấu thời gian đảm bảo rằng các lần xuất hiện lặp lại được phát hiện ngay lập tức mà không bị nhầm lẫn bởi sự chồng chéo ở mức độ cao không liên quan.
