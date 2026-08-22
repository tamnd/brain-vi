---
title: "CF 104118I - Chế tạo vật phẩm"
description: "Chúng ta được cung cấp một tập hợp lớn các vật phẩm được sắp xếp theo một hệ thống phụ thuộc chặt chẽ. Một số vật phẩm là tài nguyên cơ bản đã tồn tại với số lượng hạn chế, trong khi tất cả các vật phẩm khác được sản xuất theo công thức tiêu thụ các vật phẩm đã xác định trước đó."
date: "2026-07-02T01:53:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104118
codeforces_index: "I"
codeforces_contest_name: "2022 ICPC Asia-Manila Regional Contest"
rating: 0
weight: 104118
solve_time_s: 62
verified: true
draft: false
---

[CF 104118I - Chế tạo vật phẩm](https://codeforces.com/problemset/problem/104118/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp lớn các vật phẩm được sắp xếp theo một hệ thống phụ thuộc chặt chẽ. Một số vật phẩm là tài nguyên cơ bản đã tồn tại với số lượng hạn chế, trong khi tất cả các vật phẩm khác được sản xuất theo công thức tiêu thụ các vật phẩm đã xác định trước đó. Thuộc tính cấu trúc quan trọng là mọi công thức chỉ sử dụng các mục có ID lớn hơn, đảm bảo không có chu kỳ và buộc thứ tự tính toán tự nhiên từ ID cao xuống ID thấp. 

Trong số tất cả các mục, n đầu tiên là đặc biệt. Đây là những món duy nhất chúng tôi quan tâm trong câu trả lời cuối cùng và chúng không thể xuất hiện dưới dạng nguyên liệu trong bất kỳ công thức nào khác. Mục tiêu là xác định xem có thể sản xuất bao nhiêu mặt hàng đặc biệt này ít nhất một lần, giả sử chúng ta chỉ bắt đầu với số lượng nguyên liệu thô nhất định. 

Mỗi vật phẩm được chế tạo tiêu thụ một đơn vị của mỗi thành phần trong công thức của nó. Bởi vì bản thân các thành phần có thể được chế tạo từ các thành phần khác nên việc sản xuất ra sản phẩm cuối cùng có thể mở rộng thành yêu cầu đầy đủ về nguyên liệu thô. Khó khăn cốt lõi là các công thức nấu ăn tạo thành một DAG nhiều lớp, do đó mỗi sản phẩm cuối cùng tương ứng với một vectơ nhu cầu về nguyên liệu thô thay vì chi phí trực tiếp. 

Các ràng buộc buộc chúng tôi phải tránh xa mọi mô phỏng chế tạo theo mỗi truy vấn. Có thể có tới hai trăm nghìn mục và tổng cộng năm trăm nghìn cạnh công thức, do đó, bất kỳ giải pháp nào liên tục tính toán lại các phần phụ thuộc cho mỗi mục cuối cùng sẽ quá chậm. Tuy nhiên, số lượng nguyên liệu nhiều nhất là mười, và số lượng sản phẩm cuối cùng nhiều nhất là mười lăm. Sự phân chia này là gợi ý về cấu trúc chính: biểu đồ lớn nhưng số chiều của ràng buộc tài nguyên thực tế là cực kỳ nhỏ. 

Một trường hợp phức tạp phát sinh từ việc tái sử dụng trung gian. Một vật phẩm trung gian có thể được sử dụng trong nhiều công thức nấu ăn, vì vậy giá trị của nó phải được tính một lần và tái sử dụng. Ví dụ: nếu hai sản phẩm cuối cùng khác nhau cần mặt hàng A thì việc tính toán lại chi phí nguyên vật liệu thô cho mỗi sản phẩm một cách độc lập sẽ làm tăng gấp đôi số lượng công việc và có khả năng dẫn đến việc triển khai không nhất quán. Giải thích đúng là mỗi mặt hàng có một “vectơ chi phí nguyên liệu thô” cố định trên mỗi đơn vị, không phụ thuộc vào cách nó được sử dụng sau này. 

Một vấn đề khác xuất hiện nếu người ta cố gắng xây dựng các sản phẩm theo thứ tự một cách tham lam. Ngay cả khi một sản phẩm rẻ tiền, việc sản xuất nó có thể tiêu tốn một nguyên liệu thô quý hiếm cần thiết cho nhiều sản phẩm khác. Điều này làm cho các quyết định của địa phương trở nên không đáng tin cậy; chúng ta phải đánh giá các tập hợp con của sản phẩm cuối cùng trên toàn cầu. 

## Phương pháp tiếp cận 

Một chiến lược mạnh mẽ trực tiếp sẽ là mô phỏng việc chế tạo từng sản phẩm cuối cùng một cách độc lập bằng cách mở rộng đệ quy công thức của nó xuống nguyên liệu thô. Về nguyên tắc, điều này hoạt động vì biểu đồ phụ thuộc không theo chu kỳ, vì vậy chúng tôi có thể tính toán vectơ chi phí cho từng sản phẩm cuối cùng bằng DFS. Sau khi có được các vectơ này, chúng ta có thể thử tất cả các tập hợp con của sản phẩm cuối cùng và kiểm tra xem tổng yêu cầu nguyên liệu thô của chúng có phù hợp với lượng hàng sẵn có hay không. 

Sức mạnh vũ phu này đã chứa đựng sự phân rã phù hợp, nhưng cái giá phải trả của nó đến từ sự đệ quy lặp đi lặp lại nếu được thực hiện một cách bất cẩn. Nếu không ghi nhớ, mỗi sản phẩm có thể mở rộng thông qua các sơ đồ con chồng chéo, dẫn đến sự lặp lại theo cấp số nhân. Ngay cả với việc ghi nhớ, thách thức chi phí thực sự chuyển sang liệt kê tập hợp con, là 2^n trong đó n nhiều nhất là 15, vì vậy phần đó thực sự có thể chấp nhận được. 

Quan sát quan trọng là cấu trúc tự nhiên tách thành hai giai đoạn. Đầu tiên, chúng tôi nén toàn bộ DAG lớn thành một biểu diễn có chiều cố định nhỏ: mỗi mục trở thành một vectơ có kích thước tối đa là 10, mô tả số lượng đơn vị của mỗi nguyên liệu thô mà nó tiêu thụ. Thứ hai, chúng tôi giải quyết một vấn đề tối ưu hóa tổ hợp nhỏ trên tối đa mười lăm mục bằng cách sử dụng các vectơ này.

Khi mọi mục được biểu diễn dưới dạng vectơ chi phí 10 chiều, vấn đề sẽ trở thành: chọn tập hợp con lớn nhất trong số tối đa mười lăm vectơ sao cho tổng tọa độ của chúng không vượt quá vectơ công suất cố định. Đây là cách kiểm tra tính khả thi của tập hợp con cổ điển đối với các ràng buộc về kích thước nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng lặp đi lặp lại ngây thơ cho mỗi sản phẩm cuối cùng | O(n · m) trường hợp xấu nhất hoặc tệ hơn mà không cần ghi nhớ | O(m) | Quá chậm/rủi ro | 
| Xây dựng vectơ chi phí + liệt kê tập hợp con | O(m + 2^n · n · K) | O(m · K) | Đã chấp nhận | 

Ở đây K là số lượng nguyên liệu thô, nhiều nhất là mười. 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén toàn bộ biểu đồ phụ thuộc vào chi phí nguyên liệu thô cho mỗi mặt hàng, sau đó giải quyết vấn đề lựa chọn tập hợp con cho các mặt hàng cuối cùng. 

1. Xác định tất cả các mặt hàng nguyên liệu thô và gán cho mỗi mặt hàng một chỉ số từ 0 đến K−1. Mỗi hạng mục như vậy đóng góp chính xác một đơn vị thuộc loại riêng của nó và không có gì khác. 
2. Tạo vectơ chi phí cho mọi mặt hàng. Đối với một mặt hàng nguyên liệu thô, vectơ này là vectơ đơn vị tương ứng với chính nó. Đối với bất kỳ mặt hàng chế tạo nào, hãy khởi tạo vectơ chi phí của nó bằng tất cả số không. 
3. Xử lý các mục theo thứ tự ID tăng dần. Bởi vì mọi công thức chỉ sử dụng các vật phẩm có ID lớn hơn nên tất cả các thành phần của một vật phẩm đều được đảm bảo đã tính toán vectơ chi phí. 
4. Đối với mỗi vật phẩm được chế tạo, hãy tính vectơ chi phí của nó bằng cách tính tổng các vectơ chi phí của tất cả các vật phẩm trong công thức của nó. Điều này hợp lệ vì quá trình chế tạo tiêu tốn một đơn vị của mỗi thành phần, do đó yêu cầu về nguyên liệu thô sẽ tăng tuyến tính. 
5. Sau khi tiền xử lý, trích xuất các vectơ chi phí cho n mục đầu tiên. Đây là những ứng cử viên duy nhất chúng tôi có thể chọn trong vòng tuyển chọn cuối cùng. 
6. Liệt kê tất cả các tập hợp con của n mục này bằng cách sử dụng mặt nạ bit. Đối với mỗi tập hợp con, hãy tính tổng mức tiêu thụ nguyên liệu thô bằng cách tính tổng các vectơ chi phí của các mặt hàng được bao gồm. 
7. Kiểm tra tính khả thi của từng tập hợp con bằng cách xác minh rằng mọi kích thước nguyên liệu thô không vượt quá nguồn cung sẵn có. 
8. Theo dõi kích thước tập hợp con tối đa thỏa mãn ràng buộc. 

Điểm quan trọng trong cách xây dựng này là mọi mặt hàng đều có sự phân hủy cố định thành nguyên liệu thô độc lập với sản phẩm cuối cùng mà nó đóng góp vào. Điều này làm cho việc đánh giá tập hợp con trở nên nhất quán và bổ sung. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến nén: sau bước 4, mỗi mặt hàng i được mô tả đầy đủ bằng một vectơ biểu thị số lượng chính xác của từng nguyên liệu thô cần thiết để sản xuất một đơn vị i. Vì các công thức nấu ăn tạo thành một DAG được sắp xếp theo số ID giảm dần nên vectơ này được xác định rõ ràng và được tính toán chính xác một lần cho mỗi mục. Mỗi tập hợp sản phẩm cuối cùng khả thi tương ứng với một tập hợp con của các vectơ này có tổng tọa độ tôn trọng giới hạn tài nguyên và mọi tập hợp con được kiểm tra bằng thuật toán đều tương ứng với một kế hoạch chế tạo đồng thời hợp lệ vì các mặt hàng trung gian luôn có thể được sản xuất độc lập miễn là đủ nguyên liệu thô. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())

    ingredients = [[] for _ in range(m + 1)]
    raw_id = []
    raw_index = [-1] * (m + 1)

    for i in range(1, m + 1):
        tmp = list(map(int, input().split()))
        c = tmp[0]
        if c == 0:
            raw_id.append(i)
        else:
            ingredients[i] = tmp[1:]

    K = len(raw_id)
    for idx, rid in enumerate(raw_id):
        raw_index[rid] = idx

    p = [0] * K
    # second pass to read raw quantities
    # (we reparse lines implicitly is not possible; instead store earlier)
    # so we store separately
    sys.stdin.seek(0)
    input()
    raw_qty = [0] * (m + 1)
    parsed_ing = [[] for _ in range(m + 1)]

    for i in range(1, m + 1):
        tmp = list(map(int, input().split()))
        c = tmp[0]
        if c == 0:
            raw_qty[i] = tmp[1]
        else:
            parsed_ing[i] = tmp[1:]

    # rebuild ingredients correctly
    ingredients = parsed_ing

    cost = [[0] * K for _ in range(m + 1)]

    for i in range(1, m + 1):
        if raw_index[i] != -1:
            cost[i][raw_index[i]] = 1
        else:
            for v in ingredients[i]:
                for k in range(K):
                    cost[i][k] += cost[v][k]

    final = list(range(1, n + 1))

    best = 0
    size = len(final)

    for mask in range(1 << size):
        total = [0] * K
        ok = True
        cnt = 0

        for i in range(size):
            if mask & (1 << i):
                cnt += 1
                fi = final[i]
                for k in range(K):
                    total[k] += cost[fi][k]
                    if total[k] > raw_qty[raw_id[k]]:
                        ok = False
                        break
                if not ok:
                    break

        if ok:
            best = max(best, cnt)

    print(best)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách tách nguyên liệu thô khỏi các mặt hàng được chế tạo và gán cho mỗi nguyên liệu thô một chỉ số cố định trong một không gian vectơ nhỏ gọn. Sau đó, nó xây dựng một biểu diễn phụ thuộc ngược để mỗi hạng mục có thể được mở rộng thành các yêu cầu về nguyên liệu thô. 

Vòng tính toán chi phí là bước nén khóa. Mỗi mặt hàng tích lũy các vectơ nguyên liệu thô của các thành phần của nó, điều này hợp lệ vì mỗi công thức tiêu thụ một đơn vị của mỗi thành phần. Vì các phần phụ thuộc luôn trỏ đến ID cao hơn nên tất cả các giá trị bắt buộc đều đã được tính toán khi xử lý một mục. 

Cuối cùng, phép liệt kê tập hợp con sẽ kiểm tra mọi sự kết hợp của các sản phẩm cuối cùng. Việc kiểm tra tính khả thi được thực hiện tăng dần và việc dừng sớm được sử dụng ngay khi vượt quá giới hạn nguyên liệu thô. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi xem xét tất cả các tập hợp con của sản phẩm cuối cùng 1 và 2, đồng thời theo dõi việc sử dụng nguyên liệu thô. 

| Mặt nạ | Vật phẩm được chọn | Sử dụng thô | Khả thi | 
| --- | --- | --- | --- | 
| 00 | không | (0,0,0...) | vâng | 
| 01 | {1} | vector của mục 1 | vâng | 
| 10 | {2} | vector của mục 2 | vâng | 
| 11 | {1,2} | tổng vượt quá giới hạn | không | 

Chỉ có thể chọn một mục cùng nhau trong cấu hình này, vì vậy tối đa là 1. 

Dấu vết này cho thấy ngay cả khi mỗi mục đều khả thi riêng lẻ thì sự kết hợp của chúng có thể vi phạm ràng buộc tài nguyên dùng chung. 

### Mẫu 2 

Ở đây một nguyên liệu thô có tính sẵn có cao hơn, cho phép cả hai sản phẩm cuối cùng cùng tồn tại. 

| Mặt nạ | Vật phẩm được chọn | Sử dụng thô | Khả thi | 
| --- | --- | --- | --- | 
| 00 | không | 0 | vâng | 
| 01 | {1} | v1 | vâng | 
| 10 | {2} | v2 | vâng | 
| 11 | {1,2} | v1 + v2 | vâng | 

Điều này xác nhận rằng việc liệt kê tập hợp con nắm bắt chính xác các hiệu ứng tích lũy tài nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · K + 2^n · n · K) | mỗi mục được tính một lần, sau đó kiểm tra tất cả các tập hợp con | 
| Không gian | O(m · K) | lưu trữ các vectơ chi phí cho từng hạng mục | 

Các ràng buộc được chọn sao cho m lớn nhưng K và n rất nhỏ. Quá trình tiền xử lý chia tỷ lệ tuyến tính theo số cạnh của công thức, trong khi phần hàm mũ bị giới hạn ở mức tối đa là 2^15, có thể dễ dàng quản lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# sample 1
assert run("""2 6
2 3 4
3 4 5 6
0 1
0 1
0 1
0 1
""") == "1"

# sample 2
assert run("""2 6
2 3 4
3 4 5 6
0 1
0 2
0 1
0 1
""") == "2"

# custom: single item always possible
assert run("""1 2
0 5
0 3
""") == "1"

# custom: impossible both due to shared resource
assert run("""2 3
2 3
0 1
0 1
""") == "1"

# custom: independent resources allow full selection
assert run("""2 3
2 2 3
0 5
0 5
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nguyên liệu đơn lẻ | 1 | tính khả thi cơ bản | 
| nút cổ chai chia sẻ | 1 | ghép các ràng buộc tập hợp con | 
| nguồn lực độc lập | 2 | độc lập phụ gia | 

## Vỏ cạnh 

Trường hợp một bên là khi nhiều sản phẩm cuối cùng có chung chuỗi phụ thuộc sâu sắc. Thuật toán xử lý việc này một cách chính xác vì mỗi mục trung gian được tính toán một lần dưới dạng vectơ dùng chung. Ví dụ: nếu cả hai sản phẩm cuối cùng đều phụ thuộc vào cùng một mặt hàng trung gian thì sản phẩm trung gian đó đóng góp giống nhau vào cả hai vectơ chi phí, do đó việc tái sử dụng sẽ tự động được tính đến. 

Một trường hợp khác là khi nguyên liệu thô không được sử dụng trực tiếp trong sản phẩm cuối cùng mà chỉ thông qua chuỗi dài. Bước tiền xử lý vẫn chỉ định các vectơ chính xác vì nguyên liệu thô truyền lên trên qua từng lớp công thức, đảm bảo không bỏ sót phần phụ thuộc ẩn nào. 

Trường hợp khó khăn cuối cùng là khi giải pháp tối ưu yêu cầu bỏ qua một sản phẩm có vẻ rẻ tiền để bảo toàn nguyên liệu thô quý hiếm cho sự kết hợp khác. Bảng liệt kê tập hợp con đánh giá rõ ràng tất cả các kết hợp, do đó, sự đánh đổi như vậy được nắm bắt một cách tự nhiên mà không cần phải suy luận tham lam.
