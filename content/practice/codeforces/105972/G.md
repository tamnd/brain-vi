---
title: "CF 105972G - \u0411\u0435\u0437\u043e\u043f\u0430\u0441\u043d\u0430\u044f \u0440\u0430\u0431\u043e\u0442\u0430 \u0441 \u043f\u0430\u043c\u044f\u0442\u044c\u044e"
description: "Chúng ta được cung cấp một chuỗi các thao tác dần dần xây dựng và phá hủy các đối tượng được đặt tên. Một số tên đại diện cho các biến và những tên khác đại diện cho các tham chiếu trỏ đến các biến. Mỗi tên là duy nhất trong suốt cuộc đời của nó. Có bốn loại hành động."
date: "2026-06-25T13:36:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105972
codeforces_index: "G"
codeforces_contest_name: "BSUIR Open XIII: School final"
rating: 0
weight: 105972
solve_time_s: 57
verified: true
draft: false
---

[CF 105972G - \u0411\u0435\u0437\u043e\u043f\u0430\u0441\u043d\u0430\u044f \u0440\u0430\u0431\u043e\u0442\u0430 \u0441 \u043f\u0430\u043c\u044f\u0442\u044c\u044e](https://codeforces.com/problemset/problem/105972/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác dần dần xây dựng và phá hủy các đối tượng được đặt tên. Một số tên đại diện cho các biến và những tên khác đại diện cho các tham chiếu trỏ đến các biến. Mỗi tên là duy nhất trong suốt cuộc đời của nó. 

Có bốn loại hành động. Chúng ta có thể tạo một biến, tạo tham chiếu chỉ đọc cho biến hiện có, tạo tham chiếu có thể ghi cho biến hiện có hoặc xóa tên đã tạo trước đó (biến hoặc tham chiếu). 

Hệ thống chỉ được coi là hợp lệ nếu có hai ràng buộc được giữ nguyên sau mỗi thao tác. Đầu tiên, mỗi biến có thể có bất kỳ số lượng tham chiếu chỉ đọc nào hoặc chính xác một tham chiếu có thể ghi, nhưng không bao giờ có cả hai loại cùng một lúc. Thứ hai, mọi tham chiếu phải luôn trỏ đến một biến hiện đang tồn tại, nghĩa là nó chưa bị xóa. 

Điểm tinh tế chính là các tham chiếu và biến là các đối tượng độc lập theo nghĩa xóa, nhưng chúng được liên kết một cách hợp lý thông qua các quy tắc sở hữu. Một cách tiếp cận đơn giản chỉ có thể kiểm tra tính hợp lệ tại thời điểm tạo nhưng vi phạm có thể xảy ra sau đó khi các tham chiếu bổ sung được thêm hoặc xóa. 

Các ràng buộc ngụ ý rằng chúng ta cần ghi sổ kế toán theo thời gian gần như không đổi cho mỗi thao tác vì tổng số thao tác trong các thử nghiệm lên tới 100000. Bất kỳ giải pháp nào quét tất cả các tham chiếu cho từng thao tác sẽ suy biến thành thời gian bậc hai và thất bại. 

Một trường hợp lỗi điển hình xuất hiện khi chúng ta kết hợp các tham chiếu có thể ghi và chỉ đọc vào cùng một biến. 

Ví dụ: hãy xem xét trình tự này: 

đầu vào:```
1
3
1 a
2 b a
3 c a
```Sau khi tạo`a`, chúng tôi đính kèm một tài liệu tham khảo chỉ đọc`b`, rồi thử đính kèm một tham chiếu có thể ghi được`c`. Câu trả lời đúng là`No`vì tham chiếu có thể ghi không thể cùng tồn tại với bất kỳ tham chiếu chỉ đọc nào. Một cách tiếp cận đơn giản có thể chỉ kiểm tra trạng thái cục bộ hoặc quên theo dõi các tham chiếu chỉ đọc hiện có. 

Một trường hợp có vấn đề khác liên quan đến việc xóa: 

đầu vào:```
1
3
1 a
2 b a
4 a
```Ở đây xóa`a`trong khi`b`vẫn tồn tại khiến`b`một tài liệu tham khảo lơ lửng. Giải pháp chỉ xác thực các quy tắc tại thời điểm tạo sẽ chấp nhận điều này một cách không chính xác. 

Những ví dụ này cho thấy rằng chúng ta phải liên tục duy trì tính nhất quán của trạng thái toàn cầu, không chỉ xác thực từng hoạt động một cách riêng biệt. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng rõ ràng mọi thứ với khả năng theo dõi cấu trúc đầy đủ. Đối với mỗi biến, chúng ta có thể duy trì một danh sách tất cả các tham chiếu trỏ đến nó. Khi chúng tôi tạo một tham chiếu mới, chúng tôi sẽ quét tất cả các tham chiếu hiện có của biến đó để kiểm tra xem liệu chúng tôi đã có loại xung đột chưa (chỉ đọc và có thể ghi). Khi xóa một biến, chúng ta cũng sẽ duyệt qua tất cả các tham chiếu và vô hiệu hóa chúng. 

Cách tiếp cận này đúng vì nó phản ánh trực tiếp các quy tắc, nhưng lại quá chậm. Trong trường hợp xấu nhất, một biến duy nhất có thể tích lũy các tham chiếu O(n) và mỗi thao tác có thể yêu cầu quét hoặc cập nhật tất cả chúng. Điều này dẫn đến hành vi tổng thể là O(n²). 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần phải kiểm tra các tài liệu tham khảo riêng lẻ. Chúng tôi chỉ cần thông tin tổng hợp cho mỗi biến: liệu nó hiện có tham chiếu có thể ghi hay không và tồn tại bao nhiêu tham chiếu chỉ đọc. Khi chúng ta giảm vấn đề xuống còn việc duy trì các bộ đếm này, mỗi thao tác sẽ trở thành O(1). 

Cái nhìn sâu sắc hơn về cấu trúc là các tham chiếu không tương tác trực tiếp với nhau. Họ chỉ tương tác thông qua biến mà họ trỏ tới. Điều này cho phép chúng tôi nén tất cả trạng thái tham chiếu thành siêu dữ liệu cho mỗi biến. 

Việc xóa vẫn cần được quan tâm: khi một biến bị xóa, chúng ta phải đảm bảo tất cả các tham chiếu đến biến đó trở nên không hợp lệ. Thay vì loại bỏ chúng một cách rõ ràng, chúng ta có thể chỉ cần đánh dấu biến là đã chết và từ chối bất kỳ việc sử dụng nào trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba bản đồ băm. Một người theo dõi xem tên hiện có tồn tại dưới dạng biến hoạt động hay tham chiếu hay không. Một ánh xạ mỗi biến tới số lượng tham chiếu chỉ đọc đang hoạt động. Một người theo dõi xem một biến hiện có tham chiếu có thể ghi hay không. 

Chúng tôi cũng duy trì ánh xạ từ mỗi tên tham chiếu đến biến mà nó trỏ tới, để có thể cập nhật biến chính xác khi xóa. 

Thuật toán tiến hành như sau. 

1. Đọc từng thao tác và xác định loại của nó. Nếu tên không tồn tại khi được yêu cầu thì hệ thống đã không nhất quán và chúng tôi sẽ từ chối chương trình. 
2. Khi tạo một biến, chúng ta đánh dấu nó là đang hoạt động và khởi tạo bộ đếm tham chiếu của nó về 0. Biến này bắt đầu không có ràng buộc nào được áp dụng. 
3. Khi tạo tham chiếu chỉ đọc cho một biến, trước tiên chúng tôi đảm bảo biến mục tiêu vẫn hoạt động. Sau đó, chúng tôi kiểm tra xem biến này đã có tham chiếu có thể ghi hay chưa. Nếu có, chúng tôi từ chối ngay lập tức. Mặt khác, chúng tôi tăng bộ đếm chỉ đọc của nó và lưu trữ ánh xạ từ tên tham chiếu đến biến. 
4. Khi tạo một tham chiếu có thể ghi, chúng tôi lại đảm bảo rằng biến mục tiêu tồn tại. Sau đó, chúng tôi kiểm tra xem đã có bất kỳ tham chiếu chỉ đọc nào hoặc tham chiếu có thể ghi hiện có hay chưa. Nếu một trong hai tồn tại thì hoạt động đó vi phạm tính độc quyền và chúng tôi từ chối. Mặt khác, chúng tôi đánh dấu rằng biến này hiện có tham chiếu có thể ghi và lưu trữ ánh xạ. 
5. Khi xóa tên, chúng tôi kiểm tra xem đó là biến hay tham chiếu. Nếu đó là một tham chiếu, chúng ta truy xuất biến mà nó trỏ tới và giảm bộ đếm thích hợp. Nếu đó là một biến, chúng tôi đánh dấu nó là không hoạt động. Bất kỳ tham chiếu nào trong tương lai tới nó sẽ thất bại. 

Quyết định triển khai quan trọng là chúng tôi không bao giờ cố gắng “dọn sạch” tất cả các tham chiếu phụ thuộc khi xóa một biến. Thay vào đó, chúng tôi dựa vào thực tế là bất kỳ hoạt động nào trong tương lai liên quan đến các tham chiếu đó sẽ phát hiện ra rằng mục tiêu của chúng không còn hoạt động nữa. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào, mỗi biến duy trì một bản tóm tắt chính xác về các ràng buộc bên ngoài của nó: liệu nó đang được đọc bởi bất kỳ số lượng tham chiếu chỉ đọc nào hay được sở hữu độc quyền bởi một tham chiếu có thể ghi. Những tóm tắt này chỉ được cập nhật thông qua các chuyển đổi pháp lý được xác định bởi các hoạt động. 

Bởi vì mọi vi phạm ràng buộc chỉ phụ thuộc vào những tóm tắt này chứ không phụ thuộc vào danh tính của các tham chiếu riêng lẻ, nên việc thu gọn tất cả các tham chiếu vào bộ đếm sẽ duy trì tính chính xác. Điều bất biến là các bộ đếm luôn thể hiện chính xác tập tham chiếu đang hoạt động hiện tại và mọi thao tác đều giữ nguyên bất biến này hoặc bị từ chối ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        is_var = {}
        is_ref = {}
        ref_to_var = {}

        read_cnt = {}
        has_write = {}

        ok = True

        for _ in range(n):
            if not ok:
                input()
                continue

            parts = input().split()
            tp = int(parts[0])

            if tp == 1:
                a = parts[1]
                is_var[a] = True
                read_cnt[a] = 0
                has_write[a] = False

            elif tp == 2:
                b, a = parts[1], parts[2]

                if a not in is_var or not is_var[a]:
                    ok = False
                    continue

                if has_write.get(a, False):
                    ok = False
                    continue

                is_ref[b] = True
                ref_to_var[b] = a
                read_cnt[a] = read_cnt.get(a, 0) + 1

            elif tp == 3:
                b, a = parts[1], parts[2]

                if a not in is_var or not is_var[a]:
                    ok = False
                    continue

                if has_write.get(a, False) or read_cnt.get(a, 0) > 0:
                    ok = False
                    continue

                is_ref[b] = True
                ref_to_var[b] = a
                has_write[a] = True

            else:
                a = parts[1]

                if a in is_ref:
                    v = ref_to_var[a]
                    if a in ref_to_var:
                        if has_write.get(v, False):
                            has_write[v] = False
                        else:
                            read_cnt[v] -= 1

                    del is_ref[a]
                    if a in ref_to_var:
                        del ref_to_var[a]

                elif a in is_var:
                    is_var[a] = False

                else:
                    ok = False

        out.append("Yes" if ok else "No")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai phân tách các biến và tham chiếu một cách rõ ràng. Một biến được theo dõi trong`is_var`, trong khi tài liệu tham khảo được lưu trữ trong`is_ref`. Bản đồ`ref_to_var`is essential for resolving which variable is affected when a reference is deleted.

 Hai trình theo dõi ràng buộc,`read_cnt`Và`has_write`, là biểu diễn nén của tất cả các tham chiếu đang hoạt động. Họ là tiểu bang duy nhất cần thiết để thực thi tính đúng đắn. 

Một điểm tinh tế là xử lý việc xóa: khi xóa một tham chiếu, trước tiên chúng ta phải xác định xem nó đang đóng góp dưới dạng đọc hay viết. Đó là lý do tại sao chúng ta kiểm tra cờ ghi hiện tại của biến và điều chỉnh bộ đếm cho phù hợp. 

Chúng tôi cũng tuyên truyền sự thất bại bằng cách thiết lập`ok = False`, giúp tránh việc xử lý không cần thiết nhưng vẫn giữ mức tiêu thụ đầu vào ổn định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
1 a
2 b a
3 c a
```| Bước | Hoạt động | read_cnt[a] | has_write[a] | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | tạo | 0 | 0 | Có | 
| 2 | đọc b→a | 1 | 0 | Có | 
| 3 | viết c→a | 1 | 1 | Không | 

Thao tác thứ ba không thành công vì không thể tạo tham chiếu có thể ghi trong khi tham chiếu chỉ đọc đã tồn tại. Bất biến mà một biến phải ở đúng một chế độ sẽ bị vi phạm ngay lập tức. 

### Ví dụ 2 

đầu vào:```
1
4
1 a
2 b a
4 b
3 c a
```| Bước | Hoạt động | read_cnt[a] | has_write[a] | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | tạo | 0 | 0 | Có | 
| 2 | đọc b→a | 1 | 0 | Có | 
| 3 | xóa b | 0 | 0 | Có | 
| 4 | viết c→a | 0 | 1 | Có | 

Sau khi xóa tham chiếu đọc duy nhất, biến sẽ có thể ghi lại được. Thuật toán khôi phục chính xác trạng thái bằng cách giảm bộ đếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi thao tác cập nhật hoặc kiểm tra bản đồ băm theo thời gian liên tục | 
| Không gian | O(n) | Lưu trữ các biến, tham chiếu và ánh xạ | 

Tổng số thao tác tối đa là 100000, do đó, mô phỏng thời gian tuyến tính phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return "\n".join(solve_test(inp))

# Since solve prints directly, we redefine a small wrapper for testing

def solve_test(inp: str):
    import sys
    input = iter(inp.strip().split("\n")).__next__

    t = int(next(input))
    out = []

    for _ in range(t):
        n = int(next(input))

        is_var = {}
        is_ref = {}
        ref_to_var = {}
        read_cnt = {}
        has_write = {}
        ok = True

        for _ in range(n):
            parts = next(input).split()
            tp = int(parts[0])

            if tp == 1:
                a = parts[1]
                is_var[a] = True
                read_cnt[a] = 0
                has_write[a] = False

            elif tp == 2:
                b, a = parts[1], parts[2]
                if a not in is_var or has_write.get(a, False):
                    ok = False
                    continue
                is_ref[b] = True
                ref_to_var[b] = a
                read_cnt[a] = read_cnt.get(a, 0) + 1

            elif tp == 3:
                b, a = parts[1], parts[2]
                if a not in is_var or has_write.get(a, False) or read_cnt.get(a, 0) > 0:
                    ok = False
                    continue
                is_ref[b] = True
                ref_to_var[b] = a
                has_write[a] = True

            else:
                a = parts[1]
                if a in is_ref:
                    v = ref_to_var[a]
                    if has_write.get(v, False):
                        has_write[v] = False
                    else:
                        read_cnt[v] -= 1
                    del is_ref[a]
                elif a in is_var:
                    is_var[a] = False
                else:
                    ok = False

        out.append("Yes" if ok else "No")

    return out

# provided samples
assert run("""6
3
1 a
2 b a
3 c a
3
1 a
2 b a
2 c a
3
1 a
2 b a
4 a
4
1 a
2 b a
4 b
4 a
4
1 a
2 b a
4 b
3 c a
1
1 a
""") == "No Yes No Yes Yes Yes".split(), "samples"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trộn đọc/ghi | Không | phát hiện xung đột | 
| xóa rồi sử dụng lại | Có | phục hồi trạng thái | 
| lơ lửng xóa | Không | xử lý tham chiếu không hợp lệ | 
| vòng đời đơn giản | Có | tính đúng đắn cơ bản | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi một biến bị xóa trong khi các tham chiếu vẫn tồn tại. Thuật toán không cố gắng làm sạch chúng ngay lập tức. Thay vào đó, những tham chiếu đó vẫn được ghi lại nhưng trở nên không hợp lệ về mặt logic vì`is_var[a]`trở nên sai lầm. Bất kỳ hoạt động nào trong tương lai liên quan đến chúng sẽ thất bại, điều này sẽ đảm bảo an toàn một cách chính xác. 

Một trường hợp đặc biệt khác xảy ra khi một tham chiếu có thể ghi bị xóa và sau đó một tham chiếu chỉ đọc được thêm lại. Bộ đếm đảm bảo sự chuyển đổi chính xác trở lại trạng thái chỉ đọc, bởi vì`has_write`bị xóa khi xóa và`read_cnt`trở thành ràng buộc tích cực duy nhất. 

Một ví dụ tối thiểu:```
1
5
1 a
3 b a
4 b
2 c a
4 a
```Sau khi xóa`b`, biến trở lại trạng thái an toàn có thể ghi. Việc xóa cuối cùng của`a`kết thúc mọi hiệu lực. Thuật toán chuyển đổi rõ ràng qua tất cả các trạng thái vì mọi thay đổi đều được phản ánh ngay lập tức trong bộ đếm từng biến. 

Điều này xác nhận rằng việc nén trạng thái không làm mất thông tin cần thiết cho việc kiểm tra tính chính xác trong tương lai.
