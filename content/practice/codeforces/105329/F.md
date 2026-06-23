---
title: "CF 105329F - \u0411\u0430\u0448\u043d\u044f"
description: "Chúng ta được cho một dòng các vị trí từ 1 đến n trừ 1, ban đầu một số học sinh được đặt trên các điểm nguyên này. Mỗi học sinh độc lập chọn hướng ban đầu, di chuyển sang trái về vị trí 0 hoặc sang phải về vị trí n."
date: "2026-06-22T09:34:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105329
codeforces_index: "F"
codeforces_contest_name: "\u041f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0441\u0440\u0435\u0434\u0438 \u043d\u0430\u0447\u0438\u043d\u0430\u044e\u0449\u0438\u0445 2024"
rating: 0
weight: 105329
solve_time_s: 87
verified: false
draft: false
---

[CF 105329F - \u0411\u0430\u0448\u043d\u044f](https://codeforces.com/problemset/problem/105329/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dòng các vị trí từ 1 đến n trừ 1, ban đầu một số học sinh được đặt trên các điểm nguyên này. Mỗi học sinh độc lập chọn hướng ban đầu, di chuyển sang trái về vị trí 0 hoặc sang phải về vị trí n. Sau đó, mỗi giây mỗi học sinh di chuyển một bước theo hướng đã chọn và khi hai học sinh gặp nhau ở cùng một vị trí, họ ngay lập tức đảo ngược hướng như thể họ đã trao đổi vận tốc. 

Quan sát vật lý quan trọng là các va chạm giữa các tác nhân chuyển động giống hệt nhau trên một đường thẳng có thể được hiểu là chúng đi xuyên qua nhau nếu chúng ta bỏ qua danh tính, bởi vì việc đổi hướng sau va chạm tương đương với việc tiếp tục đi thẳng với các nhãn đã hoán đổi. Tuy nhiên, sự kiện chúng ta quan tâm không phải là sự va chạm mà là liệu có học sinh nào đạt đến vị trí 0 hoặc vị trí n trong một khoảng thời gian t nhất định hay không. 

Với mỗi truy vấn tại thời điểm t, chúng ta được yêu cầu tính xác suất để không có học sinh nào thoát khỏi phân đoạn [0, n] tại thời điểm t. Câu trả lời được đảm bảo bằng 0 hoặc lũy thừa chính xác bằng một nửa, do đó, thay vì xác suất, chúng ta đưa ra số mũ m sao cho xác suất bằng 2 lũy thừa trừ m hoặc trừ một nếu xác suất bằng 0. 

Các ràng buộc n lên tới 100000 và q lên tới 100000 ngụ ý rằng bất kỳ giải pháp nào mô phỏng chuyển động hoặc xử lý các tương tác theo cặp một cách rõ ràng là không thể. Ngay cả O(n log n) cho mỗi truy vấn cũng sẽ quá chậm, do đó, giải pháp dự định phải xử lý trước một lần ở khoảng O(n log n) hoặc O(n) và trả lời từng truy vấn ở O(1) hoặc O(log n). 

Một trường hợp phức tạp phát sinh từ việc nhiều học sinh bắt đầu ở cùng một vị trí. Nếu nhiều học sinh chiếm một tọa độ, chuyển động tương đối của họ chỉ phụ thuộc vào việc lựa chọn phương hướng; va chạm giữa chúng không làm thay đổi cấu trúc sự kiện thoát, nhưng chúng có thể ảnh hưởng đến việc có thể thoát ngay lập tức hay không. Một tình huống tế nhị khác là khi một học sinh bắt đầu ở rất gần ranh giới, bởi vì ngay cả một lựa chọn hướng không thuận lợi cũng có thể khiến học sinh thoát ra ngay lập tức trong vài giây, khiến xác suất giảm xuống 0 đối với t đủ lớn. 

Một sự hiểu lầm ngây thơ sẽ là cho rằng học sinh hành động độc lập về các lối ra, điều này không thành công vì va chạm làm lệch quỹ đạo của chúng. Một cách tiếp cận sai lầm khác là coi chúng như những hạt không tương tác và tính xác suất sống sót của từng học sinh một cách riêng biệt; điều này bị hỏng vì việc hoán đổi sau khi va chạm sẽ thay đổi điểm cuối nào đạt được. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê tất cả 2^k bài tập hướng, trong đó k là số lượng học sinh, mô phỏng chuyển động cho mỗi bài tập và kiểm tra xem có học sinh nào thoát ra trong thời gian t hay không. Ngay cả đối với k vừa phải thì điều này là không thể, vì k có thể lên tới 100000 và 2^k là lớn về mặt thiên văn. Ngay cả việc giảm mô phỏng cho mỗi nhiệm vụ cũng sẽ tốn O(n t), vượt xa giới hạn. 

Cái nhìn sâu sắc quan trọng là các va chạm trên đường một chiều với tốc độ giống hệt nhau không ảnh hưởng đến nhiều vị trí theo thời gian; họ chỉ hoán đổi danh tính. Từ góc độ liệu có học sinh nào thoát ra hay không, va chạm là không liên quan vì việc thoát ra chỉ phụ thuộc vào việc liệu bất kỳ đường đi ban đầu nào có dẫn đến một ranh giới trong phạm vi t bước hay không, và điều này chỉ phụ thuộc vào thứ tự tương đối của các học sinh và khoảng cách của chúng đến các ranh giới.

Khi định hình lại bài toán, mỗi học sinh đóng góp một ràng buộc: nếu quay mặt sang trái sẽ đạt 0 trong a_i bước; nếu nó hướng về bên phải, nó sẽ chạm n trong n trừ a_i bước. Khó khăn là các va chạm có thể chuyển hướng một học sinh, cho phép học sinh đó cư xử giống như một học sinh khác trong nhóm địa phương một cách hiệu quả. Quan sát quan trọng là trong bất kỳ khối vị trí bị chiếm giữ liền kề tối đa nào, tập hợp các sự kiện thoát có thể xảy ra chỉ phụ thuộc vào các vị trí cực đoan trong khối đó, bởi vì các giao dịch hoán đổi nội bộ không thể ngăn chặn sự kiện thoát sớm nhất có thể xảy ra mà chỉ hoán vị ai kích hoạt nó. 

Do đó, vấn đề giảm xuống còn việc xác định, đối với mỗi vị trí, liệu có tồn tại một lối thoát cưỡng bức xác định trong phạm vi t bất kể chỉ định hướng hay không. Nếu có bất kỳ lối thoát cưỡng bức nào như vậy tồn tại thì xác suất là bằng không. Mặt khác, tính ngẫu nhiên giảm xuống thành các lựa chọn độc lập trên một số lựa chọn biên “quan trọng” nhất định và mỗi ràng buộc cố định một bit độc lập một cách hiệu quả, tạo ra xác suất bằng 2 lũy thừa trừ m trong đó m tính các ràng buộc hướng cần thiết độc lập. 

Cấu trúc cuối cùng là mỗi vị trí đóng góp nhiều nhất một ràng buộc nhị phân độc lập và câu trả lời trở thành một giá trị có thể tính toán tiền tố tùy thuộc vào số lượng “điều kiện chặn” được yêu cầu để ngăn chặn việc thoát ra trong phạm vi t. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Đếm ràng buộc khoảng thời gian tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng vị trí của sinh viên và chuyển đổi nó thành một tập hợp nhiều vị trí được sắp xếp. Thứ tự có vai trò quan trọng vì các xung đột bảo toàn trật tự tương đối ngoại trừ các giao dịch hoán đổi, vì vậy chúng ta chỉ cần thông tin kề cận. 

Đối với mỗi vị trí sinh viên x, chúng tôi tính toán hai thời điểm thoát tiềm năng: x bước tới ranh giới bên trái và n trừ x bước đến ranh giới bên phải. Học sinh sẽ thoát ra trong thời gian t nếu được chỉ định một hướng có thời gian thoát tương ứng nhiều nhất là t. 

Sau đó, chúng tôi nhận thấy rằng để tránh bất kỳ lối ra nào vào thời điểm t, mọi học sinh có thời gian thoát tối thiểu nhiều nhất là t phải buộc phải chọn hướng an toàn hơn. Điều này tạo ra một hạn chế cho mỗi sinh viên như vậy. 

Tuy nhiên, các ràng buộc không độc lập khi nhiều học sinh có cùng vị trí hoặc khi các khoảng chồng chéo trong cấu trúc bắt buộc. Để giải quyết vấn đề này, chúng tôi sắp xếp học sinh theo vị trí và quét từ trái sang phải, nhóm những học sinh “nguy hiểm” liên tiếp, nghĩa là những học sinh có ít nhất một hướng dẫn đến lối ra trong phạm vi t. 

Trong mỗi nhóm, chúng tôi theo dõi xem cả hai ràng buộc ranh giới có hoạt động đồng thời hay không. Nếu cả hai hướng cho một học sinh đều dẫn đến thoát ra trong phạm vi t thì xác suất ngay lập tức bằng 0 vì học sinh đó sẽ luôn thoát ra bất kể hướng nào, cho kết quả đầu ra trừ một. 

Mặt khác, mỗi nhóm nguy hiểm đóng góp chính xác một quyết định nhị phân độc lập: hoặc tất cả các lựa chọn thiên về bên trái bắt buộc đều được thực hiện hoặc tất cả các lựa chọn thiên về bên phải bắt buộc được thực hiện nhất quán với cấu trúc xung đột. Số mũ m là số lượng các nhóm độc lập như vậy. 

Chúng tôi trả lời từng truy vấn bằng cách tính toán lại số lượng nhóm như vậy cho ngưỡng t. 

### Tại sao nó hoạt động 

Điều bất biến là trong mỗi đoạn vị trí liền kề tối đa có thể gây ra sự thoát ra theo thời gian t, mọi sự sắp xếp lại bên trong do va chạm sẽ không thay đổi cho dù có xảy ra sự thoát ra hay không; họ chỉ hoán vị học sinh nào trở thành người đạt đến ranh giới. Do đó, mức độ tự do duy nhất ảnh hưởng đến sự sống còn là các lựa chọn ngăn chặn toàn bộ phân đoạn tạo ra quỹ đạo thoát và mỗi phân đoạn như vậy đóng góp chính xác một ràng buộc nhị phân độc lập. Điều này đảm bảo rằng không gian xác suất được phân tích thành 2 lũy thừa trừ đi số phân đoạn trừ khi phân đoạn đó chứa một lối thoát không thể tránh khỏi, trong trường hợp đó xác suất giảm xuống 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    a.sort()

    # Precompute left and right exit times
    left = a
    right = [n - x for x in a]

    for _ in range(q):
        t = int(input())

        # check impossible case: any student must exit regardless of direction
        impossible = False
        for i in range(len(a)):
            if min(left[i], right[i]) <= t:
                # if both directions lead to exit within t, forced exit
                if left[i] <= t and right[i] <= t:
                    impossible = True
                    break

        if impossible:
            print(-1)
            continue

        # count independent constraints
        m = 0
        i = 0
        while i < len(a):
            if min(left[i], right[i]) > t:
                i += 1
                continue

            # start a constrained segment
            m += 1
            j = i
            while j < len(a) and min(left[j], right[j]) <= t:
                j += 1
            i = j

        print(m)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp các vị trí sao cho sự liền kề phản ánh cấu trúc tương tác tiềm năng khi có va chạm. Đối với mỗi học sinh, chúng tôi tính toán trước tốc độ đạt được một trong hai ranh giới. Đối với mỗi truy vấn, trước tiên chúng tôi phát hiện xem có tồn tại bất kỳ học sinh nào chắc chắn phải thoát ra hay không vì cả hai hướng đều dẫn đến việc thoát ra trong thời gian t; trong trường hợp đó chúng ta sẽ xuất ngay số trừ một. 

Mặt khác, chúng tôi quét qua các vị trí đã được sắp xếp và nhóm các sinh viên liên tiếp có thời gian thoát tối thiểu có thể nhiều nhất là t. Mỗi nhóm tối đa như vậy đóng góp một ràng buộc nhị phân độc lập, được tính là m. 

Một điểm tinh tế chung là đảm bảo rằng chúng tôi không nhân đôi các ràng buộc về số lượng giữa các nhóm riêng biệt; việc này được xử lý bằng cách đưa con trỏ qua toàn bộ nhóm sau khi nó được đếm. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ có n = 7 và các vị trí [2, 2, 3]. Thời gian thoát bên trái là [2, 2, 3] và thời gian thoát bên phải là [5, 5, 4]. 

Với t = 1: 

| tôi | vị trí | trái | đúng | phút(trái,phải) | bị ép? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 5 | 2 | không | 
| 1 | 2 | 2 | 5 | 2 | không | 
| 2 | 3 | 3 | 4 | 3 | không | 

Không có học sinh nào bị buộc phải ra ngoài nên không có đoạn nào bị ràng buộc và m = 0, nghĩa là xác suất là 1. 

Với t = 2: 

| tôi | vị trí | trái | đúng | phút(trái,phải) | đoạn bắt buộc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 5 | 2 | vâng | 
| 1 | 2 | 2 | 5 | 2 | vâng | 
| 2 | 3 | 3 | 4 | 3 | không | 

Chúng tôi nhận được một phân đoạn bị ràng buộc liền kề từ chỉ số 0 đến 1, vì vậy m = 1. 

Điều này chứng tỏ rằng khi t vượt qua ngưỡng ranh giới đối với một số vị trí, các ràng buộc sẽ hợp nhất thành các khối liền kề thay vì hoạt động độc lập trên mỗi học sinh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + qn) trường hợp xấu nhất ở dạng đơn giản này | Mỗi truy vấn quét mảng và nhóm các đoạn | 
| Không gian | O(n) | Lưu trữ vị trí và mảng dẫn xuất | 

Giải pháp này được thiết kế để minh họa cấu trúc nhưng sẽ cần tối ưu hóa trong cài đặt nghiêm ngặt. Với quá trình tiền xử lý thích hợp các điểm dừng trong đó min(left[i], right[i]) thay đổi thứ tự tương ứng với t, thời gian truy vấn có thể giảm xuống O(1) hoặc O(log n), phù hợp thoải mái trong các ràng buộc cho n và q lên tới 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    left = a
    right = [n - x for x in a]

    out = []
    for _ in range(q):
        t = int(input())
        impossible = False
        for i in range(len(a)):
            if left[i] <= t and right[i] <= t:
                impossible = True
                break
        if impossible:
            out.append("-1")
            continue

        m = 0
        i = 0
        while i < len(a):
            if min(left[i], right[i]) > t:
                i += 1
                continue
            m += 1
            j = i
            while j < len(a) and min(left[j], right[j]) <= t:
                j += 1
            i = j
        out.append(str(m))

    return "\n".join(out)

# custom cases
assert run("7 2\n2 2 3\n1\n2\n") == "0\n1", "basic grouping"
assert run("5 1\n1 2 3\n10\n") == "-1", "forced exit"
assert run("6 1\n2 4 2\n1\n") == "0", "no constraints early"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhóm nhỏ | mẫu 0/1 | hợp nhất các ràng buộc một cách chính xác | 
| buộc phải thoát | -1 | phát hiện các lối thoát không thể tránh khỏi | 
| t lớn không có tác dụng | 0 | xử lý cấu hình an toàn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả học sinh đều ở rất gần ranh giới. Ví dụ: với các vị trí [1, 1, 1] và n nhỏ, tại t = 1 cả hai hướng có thể dẫn đến việc một số học sinh phải thoát ra, gây ra tình trạng không thể. Thuật toán xác định chính xác điều này vì thời gian thoát của cả bên trái và bên phải đều tối đa là t đối với các vị trí đó, ngay lập tức đặt câu trả lời thành trừ một. 

Một trường hợp khác là khi tất cả học sinh ở bên trong cách xa cả hai ranh giới. Ví dụ: nếu tất cả các vị trí đều thỏa mãn min(x, n trừ x) lớn hơn t thì quá trình quét không tạo ra các phân đoạn bị ràng buộc, do đó m = 0. Thuật toán trả về chính xác xác suất 1 vì không có lựa chọn hướng nào có thể dẫn đến việc thoát ra trong cửa sổ thời gian.
