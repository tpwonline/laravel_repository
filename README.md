# laravel_repository
工作中总结的简单易用的laravel框架的repository层，用于数据库增删改查，通用性强。

```
class ClassName
{
    private $table = '数据表名';
    /**
     * 列表
     * @param $select
     * @param $where
     * @param $where_arr
     * @param $sort mixed 排序字段（为空不进行排序）
     * @param $is_paginate mixed 是否分页（0：不分页，1：分页，2：分页（带页码））
     * @param $page_num
     * @return mixed
     */
    public function listing($select,$where,$where_arr,$sort,$is_paginate,$page_num){
        $obj = DB::table($this->table)
            ->selectRaw($select)
            ->whereRaw($where,$where_arr);
        if(!empty($sort)){  //进行排序
            $obj = $obj->orderByRaw($sort);
        }
        if($is_paginate == 0){  //不分页
            $obj = $obj->get();
        }elseif($is_paginate == 1){  //分页
            $obj = $obj->simplePaginate($page_num);
        }else{  //分页（带页码）
            $obj = $obj->paginate($page_num);
        }
        return $obj;
    }
    /**
     * 详情
     * @param $select
     * @param $where
     * @param $where_arr
     * @return mixed
     */
    public function detail($select,$where,$where_arr){
        return DB::table($this->table)->selectRaw($select)->whereRaw($where,$where_arr)->first();
    }
    /**
     * 新增
     * @param $param
     * @return mixed
     */
    public function create($param){
        $time = time();
        $param['created_at'] = $time;
        $param['updated_at'] = $time;
        return DB::table($this->table)->insertGetId($param);
    }
    /**
     * 修改
     * @param $where
     * @param $where_arr
     * @param $param
     * @return mixed
     */
    public function update($where,$where_arr,$param){
        $param['updated_at'] = time();
        return DB::table($this->table)->whereRaw($where,$where_arr)->update($param);
    }
    /**
     * 删除
     * @param $where
     * @param $where_arr
     * @return mixed
     */
    public function delete($where,$where_arr){
        return DB::table($this->table)->whereRaw($where,$where_arr)->delete();
    }
}
```
