<template>
	<div class="main-content" :style='{"padding":" 30px 20px","flexWrap":"wrap","background":"none","display":"flex"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<el-form class="center-form-pv" :style='{"padding":"0px","margin":"0 20px 0 0","background":"none","display":"flex","width":"100px","justifyContent":"space-between","order":"1"}' :inline="true" :model="searchForm">
				<el-row :style='{"padding":"0","borderColor":"#fde2db #fbccc0 #f58c71","margin":"0","textAlign":"center","display":"flex","justifyContent":"center","top":"100px","flexWrap":"wrap","left":"20px","background":"none","borderWidth":"0px","width":"calc(100% - 20px)","position":"absolute","borderStyle":"solid"}' >
					<div :style='{"width":"auto","margin":"0 10px 0 0","background":"none","display":"inline-block"}'>
						<label :style='{"padding":"0 4px 0 0","color":"#666","textAlign":"right","display":"inline-block","lineHeight":"40px","fontSize":"14px","fontWeight":"500","height":"40px"}' class="item-label">科目名称</label>
						<el-input v-model="searchForm.kemumingcheng" placeholder="科目名称" clearable></el-input>
					</div>
					<div :style='{"width":"auto","margin":"0 10px 0 0","background":"none","display":"inline-block"}' class="select">
						<label :style='{"padding":"0 4px 0 0","color":"#666","textAlign":"right","display":"inline-block","lineHeight":"40px","fontSize":"14px","fontWeight":"500","height":"40px"}' class="item-label">是否通过</label>
						<el-select  @change="sfshChange" clearable v-model="searchForm.sfsh" placeholder="是否通过">
							<el-option v-for="(item,index) in sfshOptions" v-bind:key="index" :label="item" :value="item"></el-option>
						</el-select>
					</div>
					<el-button :style='{"border":"1px solid #5c7da0","cursor":"pointer","padding":"0px","boxShadow":"inset 0px 0px 56px 0px #466c95","margin":"0px 0 0 0px","color":"#fff","minWidth":"80px","outline":"none","borderRadius":"4px","background":"#fff","width":"auto","fontSize":"14px","height":"40px"}' type="success" @click="search()">查询</el-button>
				</el-row>

				<el-row :style='{"padding":"0","margin":"110px 0 0","flexWrap":"wrap","background":"none","display":"block","width":"100px","justifyContent":"center"}'>
					<el-button :style='{"cursor":"pointer","padding":"0 0px","margin":"0 0px 16px 0","borderColor":"#812d58","color":"#333","minWidth":"100px","outline":"none","borderRadius":"0px","background":"linear-gradient(180deg, rgba(255,255,255,.1) 0%, rgba(201,238,222,.1) 58%, rgba(113,190,157,.3) 60%, rgba(93,174,139,.3) 100%)","borderWidth":"0px","width":"auto","fontSize":"14px","borderStyle":"solid","height":"40px"}' v-if="isAuth('kaoshibaoming','新增')" type="success" @click="addOrUpdateHandler()">新增</el-button>
					<el-button :style='{"cursor":"pointer","padding":"0 0px","margin":"0 0px 16px 0","borderColor":"#f07062","color":"#333","minWidth":"100px","outline":"none","borderRadius":"0px","background":"linear-gradient(180deg, rgba(255,255,255,1) 0%, rgba(255,221,221,.1) 58%, rgba(255,150,150,.3) 60%, rgba(255,118,118,.3) 100%)","borderWidth":"0px","width":"auto","fontSize":"14px","borderStyle":"dashed","height":"40px"}' v-if="isAuth('kaoshibaoming','删除')" :disabled="dataListSelections.length <= 0" type="danger" @click="deleteHandler()">删除</el-button>


					<el-button :style='{"cursor":"pointer","padding":"0 0px","margin":"0 0px 16px 0","borderColor":"#f07062","color":"#333","minWidth":"100px","outline":"none","borderRadius":"0px","background":"linear-gradient(180deg, rgba(255,255,255,1) 0%, rgba(255,221,221,.1) 58%, rgba(255,150,150,.3) 60%, rgba(255,118,118,.3) 100%)","borderWidth":"0px","width":"auto","fontSize":"14px","borderStyle":"dashed","height":"40px"}' v-if="isAuth('kaoshibaoming','审核')" :disabled="dataListSelections.length <= 0" type="danger" @click="shBatchDialog()">批量审核</el-button>


				</el-row>
			</el-form>
			
			<!-- <div> -->
				<el-table class="tables"
					:stripe='true'
					:style='{"padding":"0","borderColor":"#b4e8d2","margin":"110px 0px 20px 0px","borderRadius":"0px","borderWidth":"1px 8px 1px 8px","background":"rgba(255,255,255,.6)","flex":"1","width":"100%","borderStyle":"solid","order":"3"}' 
					v-if="isAuth('kaoshibaoming','查看')"
					:data="dataList"
					v-loading="dataListLoading"
				@selection-change="selectionChangeHandler">
					<el-table-column :resizable='true' type="selection" align="center" width="50"></el-table-column>
					<el-table-column :resizable='true' :sortable='true' label="序号" type="index" width="50" />
					<el-table-column :resizable='true' :sortable='true'  
						prop="kaoshibianhao"
					label="考试编号">
						<template slot-scope="scope">
							{{scope.row.kaoshibianhao}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="kemumingcheng"
					label="科目名称">
						<template slot-scope="scope">
							{{scope.row.kemumingcheng}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="kaoshishijian"
					label="考试时间">
						<template slot-scope="scope">
							{{scope.row.kaoshishijian}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="kaoshididian"
					label="考试地点">
						<template slot-scope="scope">
							{{scope.row.kaoshididian}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="baomingfeiyong"
					label="报名费用">
						<template slot-scope="scope">
							{{scope.row.baomingfeiyong}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="zhuyishixiang"
					label="注意事项">
						<template slot-scope="scope">
							{{scope.row.zhuyishixiang}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="jiaoshizhanghao"
					label="教师账号">
						<template slot-scope="scope">
							{{scope.row.jiaoshizhanghao}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="jiaoshixingming"
					label="监考老师">
						<template slot-scope="scope">
							{{scope.row.jiaoshixingming}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="kaoshishenqing"
					label="考试申请">
						<template slot-scope="scope">
							{{scope.row.kaoshishenqing}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="baomingshijian"
					label="报名时间">
						<template slot-scope="scope">
							{{scope.row.baomingshijian}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="xuehao"
					label="学号">
						<template slot-scope="scope">
							{{scope.row.xuehao}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="xueshengxingming"
					label="学生姓名">
						<template slot-scope="scope">
							{{scope.row.xueshengxingming}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true'  
						prop="shoujihaoma"
					label="手机号码">
						<template slot-scope="scope">
							{{scope.row.shoujihaoma}}
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true' prop="touxiang" width="200" label="头像">
						<template slot-scope="scope">
							<div v-if="scope.row.touxiang">
								<img v-if="scope.row.touxiang.substring(0,4)=='http'" :src="scope.row.touxiang.split(',')[0]" width="100" height="100">
								<img v-else :src="$base.url+scope.row.touxiang.split(',')[0]" width="100" height="100">
							</div>
							<div v-else>无图片</div>
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true' prop="ispay" label="是否支付">
						<template slot-scope="scope">
							<span style="margin-right:10px">{{scope.row.ispay=='已支付'?'已支付':'未支付'}}</span>
							<el-button v-if="scope.row.ispay!='已支付' && isAuth('kaoshibaoming','支付') " type="text" size="small" @click="payHandler(scope.row)">支付</el-button>
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true' prop="shhf" label="审核回复"></el-table-column>
					<el-table-column :resizable='true' :sortable='true' prop="sfsh" label="审核状态">
						<template slot-scope="scope">
							<span style="margin-right:10px" v-if="scope.row.sfsh=='是'">通过</span>
							<span style="margin-right:10px" v-if="scope.row.sfsh=='否'">未通过</span>
							<span style="margin-right:10px" v-if="scope.row.sfsh=='待审核'">待审核</span>
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true' v-if="isAuth('kaoshibaoming','审核')" prop="sfsh" label="审核">
						<template slot-scope="scope">
							<el-button  type="text" size="small" @click="shDialog(scope.row)">审核</el-button>
						</template>
					</el-table-column>
					<el-table-column width="300" label="操作">
						<template slot-scope="scope">
							<el-button :style='{"cursor":"pointer","padding":"0 0px","margin":"0 10px 10px","borderColor":"#5dae8b","color":"#268e61","outline":"none","borderRadius":"0px","background":"url()","borderWidth":"0 0 4px","width":"auto","fontSize":"14px","borderStyle":"double","textShadow":"0 0px 0 rgba(0,0,0,.25)","height":"32px"}' v-if=" isAuth('kaoshibaoming','查看')" type="success" size="mini" @click="addOrUpdateHandler(scope.row.id,'info')">详情</el-button>
							<el-button :style='{"cursor":"pointer","padding":"0 0px","margin":"0 10px 10px","borderColor":"#5dae8b","color":"#268e61","outline":"none","borderRadius":"0px","background":"url()","borderWidth":"0 0 4px","width":"auto","fontSize":"14px","borderStyle":"double","textShadow":"0 0px 0 rgba(0,0,0,.25)","height":"32px"}' v-if="isAuth('kaoshibaoming','准考证')" type="success" size="mini" @click="zhunkaozhengCrossAddOrUpdateHandler(scope.row,'cross','是','','')">准考证</el-button>
							<el-button :style='{"cursor":"pointer","padding":"0 0px","margin":"0 10px 10px","borderColor":"#1b89ef","color":"#1b89ef","outline":"none","borderRadius":"0px","background":"none","borderWidth":"0 0 4px","width":"auto","fontSize":"14px","borderStyle":"double","textShadow":"0 0px 0 rgba(0,0,0,.25)","height":"32px"}' v-if=" isAuth('kaoshibaoming','修改')" type="primary" size="mini" @click="addOrUpdateHandler(scope.row.id)">修改</el-button>





							<el-button :style='{"cursor":"pointer","padding":"0 0px","margin":"0 10px 10px","borderColor":"#eb4a5f","color":"#eb4a5f","outline":"none","borderRadius":"0px","background":"none","borderWidth":"0 0 4px","width":"auto","fontSize":"14px","borderStyle":"double","textShadow":"0 0px 0 rgba(0,0,0,.25)","height":"32px"}' v-if="isAuth('kaoshibaoming','删除') " type="danger" size="mini" @click="deleteHandler(scope.row.id)">删除</el-button>
						</template>
					</el-table-column>
				</el-table>
				<el-pagination
					@size-change="sizeChangeHandle"
					@current-change="currentChangeHandle"
					:current-page="pageIndex"
					background
					:page-sizes="[10, 20, 30, 50]"
					:page-size="pageSize"
					:layout="layouts.join()"
					:total="totalPage"
					prev-text="<"
					next-text=">"
					:hide-on-single-page="true"
					:style='{"padding":"0 40px","margin":"20px 0 0","whiteSpace":"nowrap","color":"#eee","textAlign":"center","width":"100%","fontWeight":"500","order":"5"}'
				></el-pagination>
			<!-- </div> -->
		</template>
		
		<!-- 添加/修改页面  将父组件的search方法传递给子组件-->
		<add-or-update v-if="addOrUpdateFlag" :parent="this" ref="addOrUpdate"></add-or-update>

		<zhunkaozheng-cross-add-or-update v-if="zhunkaozhengCrossAddOrUpdateFlag" :parent="this" ref="zhunkaozhengCrossaddOrUpdate"></zhunkaozheng-cross-add-or-update>

		<el-dialog title="审核" :visible.sync="sfshVisiable" width="50%">
			<el-form ref="form" :model="form" label-width="80px">
				<el-form-item label="审核状态">
					<el-select v-model="shForm.sfsh" placeholder="审核状态">
						<el-option label="通过" value="是"></el-option>
						<el-option label="不通过" value="否"></el-option>
						<el-option label="待审核" value="待审核"></el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="内容">
					<el-input type="textarea" :rows="8" v-model="shForm.shhf"></el-input>
				</el-form-item>
			</el-form>
			<span slot="footer" class="dialog-footer">
				<el-button @click="shDialog">取 消</el-button>
				<el-button type="primary" @click="shHandler">确 定</el-button>
			</span>
		</el-dialog>
		<el-dialog title="批量审核" :visible.sync="sfshBatchVisiable" width="50%">
			<el-form ref="form" :model="form" label-width="80px">
				<el-form-item label="审核状态">
					<el-select v-model="shBatchForm.sfsh" placeholder="审核状态">
						<el-option label="通过" value="是"></el-option>
						<el-option label="不通过" value="否"></el-option>
						<el-option label="待审核" value="待审核"></el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="内容">
					<el-input type="textarea" :rows="8" v-model="shBatchForm.shhf"></el-input>
				</el-form-item>
			</el-form>
			<span slot="footer" class="dialog-footer">
				<el-button @click="shBatchDialog">取 消</el-button>
				<el-button type="primary" @click="shBatchHandler">确 定</el-button>
			</span>
		</el-dialog>



	</div>
</template>

<script>
//$graphType1
//$buttonName1
//$subNameList1
import axios from 'axios'
import AddOrUpdate from "./add-or-update";
import zhunkaozhengCrossAddOrUpdate from "../zhunkaozheng/add-or-update";
export default {
  data() {
    return {
      searchForm: {
        key: ""
      },
      form:{},
      dataList: [],
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
      dataListLoading: false,
      dataListSelections: [],
      showFlag: true,
      sfshVisiable: false,
      shForm: {},
      sfshBatchVisiable: false,
      shBatchForm: {
        sfsh:'',
        shhf:''
      },
      batchIds:[], 
      chartVisiable: false,
      chartVisiable1: false,
      chartVisiable2: false,
      chartVisiable3: false,
      chartVisiable4: false,
      chartVisiable5: false,
      addOrUpdateFlag:false,
      zhunkaozhengCrossAddOrUpdateFlag: false,
      layouts: ["total","prev","pager","next","sizes","jumper"],

    };
  },
  created() {
    this.init();
    this.getDataList();
    this.contentStyleChange()
  },
  mounted() {
  },
  filters: {
    htmlfilter: function (val) {
      return val.replace(/<[^>]*>/g).replace(/undefined/g,'');
    }
  },
  components: {
    AddOrUpdate,
    zhunkaozhengCrossAddOrUpdate,
  },
  methods: {

    contentStyleChange() {
      this.contentPageStyleChange()
    },
    // 分页
    contentPageStyleChange(){
      let arr = []

      // if(this.contents.pageTotal) arr.push('total')
      // if(this.contents.pageSizes) arr.push('sizes')
      // if(this.contents.pagePrevNext){
      //   arr.push('prev')
      //   if(this.contents.pagePager) arr.push('pager')
      //   arr.push('next')
      // }
      // if(this.contents.pageJumper) arr.push('jumper')
      // this.layouts = arr.join()
      // this.contents.pageEachNum = 10
    },

    zhunkaozhengCrossAddOrUpdateHandler(row,type,crossOptAudit,statusColumnName,tips,statusColumnValue){
	if(crossOptAudit=='是'&&row.sfsh!='是') {
	    this.$message({
	      message: "请审核通过后再操作",
	      type: "success",
	      duration: 1500,
	      onClose: () => {
	      }
	    });
		return
	}
      this.showFlag = false;
      this.addOrUpdateFlag = false;
      this.zhunkaozhengCrossAddOrUpdateFlag = true;
      this.$storage.set('crossObj',row);
      this.$storage.set('crossTable','kaoshibaoming');
      this.$storage.set('statusColumnName',statusColumnName);
      this.$storage.set('statusColumnValue',statusColumnValue);
      this.$storage.set('tips',tips);
	if(statusColumnName!=''&&!statusColumnName.startsWith("[")) {
		var obj = this.$storage.getObj('crossObj');
		for (var o in obj){
		  if(o==statusColumnName && obj[o]==statusColumnValue){
		    this.$message({
		      message: tips,
		      type: "success",
		      duration: 1500,
		      onClose: () => {
			this.getDataList();
		      }
		    });
		      this.showFlag = true;
		      this.zhunkaozhengCrossAddOrUpdateFlag = false;
			return;
		  }
		}
	}
      this.$nextTick(() => {
      this.$refs.zhunkaozhengCrossaddOrUpdate.init(row.id,type);
      });
    },
    payHandler(row){
      this.$storage.set('paytable','kaoshibaoming');
      this.$storage.set('payObject',row);
      this.$router.push('pay');
    },







    init () {
        this.sfshOptions = "是,否,待审核".split(',');
    },
    search() {
      this.pageIndex = 1;
      this.getDataList();
    },

    // 获取数据列表
    getDataList() {
      this.dataListLoading = true;
      let params = {
        page: this.pageIndex,
        limit: this.pageSize,
        sort: 'id',
        order: 'desc',
      }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
           if(this.searchForm.kemumingcheng!='' && this.searchForm.kemumingcheng!=undefined){
            params['kemumingcheng'] = '%' + this.searchForm.kemumingcheng + '%'
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
          if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
            params['sfsh'] = this.searchForm.sfsh
          }
      this.$http({
        url: "kaoshibaoming/page",
        method: "get",
        params: params
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.dataList = data.data.list;
          this.totalPage = data.data.total;
        } else {
          this.dataList = [];
          this.totalPage = 0;
        }
        this.dataListLoading = false;
      });
    },
    // 每页数
    sizeChangeHandle(val) {
      this.pageSize = val;
      this.pageIndex = 1;
      this.getDataList();
    },
    // 当前页
    currentChangeHandle(val) {
      this.pageIndex = val;
      this.getDataList();
    },
    // 多选
    selectionChangeHandler(val) {
      this.dataListSelections = val;
    },
    // 添加/修改
    addOrUpdateHandler(id,type) {
      this.showFlag = false;
      this.addOrUpdateFlag = true;
      this.crossAddOrUpdateFlag = false;
      if(type!='info'){
        type = 'else';
      }
      this.$nextTick(() => {
        this.$refs.addOrUpdate.init(id,type);
      });
    },
    // 审核窗口
    shDialog(row){
      this.sfshVisiable = !this.sfshVisiable;
      if(row){
        this.shForm = {
          kaoshibianhao: row.kaoshibianhao,
          kemumingcheng: row.kemumingcheng,
          kaoshishijian: row.kaoshishijian,
          kaoshididian: row.kaoshididian,
          baomingfeiyong: row.baomingfeiyong,
          zhuyishixiang: row.zhuyishixiang,
          jiaoshizhanghao: row.jiaoshizhanghao,
          jiaoshixingming: row.jiaoshixingming,
          kaoshishenqing: row.kaoshishenqing,
          baomingshijian: row.baomingshijian,
          xuehao: row.xuehao,
          xueshengxingming: row.xueshengxingming,
          shoujihaoma: row.shoujihaoma,
          touxiang: row.touxiang,
          sfsh: row.sfsh,
          shhf: row.shhf,
          ispay: row.ispay,
          crossuserid: row.crossuserid,
          crossrefid: row.crossrefid,
          id: row.id
        }
      }
    },
    // 审核
    shHandler(){
      this.$confirm(`确定操作?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$http({
          url: "kaoshibaoming/update",
          method: "post",
          data: this.shForm
        }).then(({ data }) => {
          if (data && data.code === 0) {
            this.$message({
              message: "操作成功",
              type: "success",
              duration: 1500,
              onClose: () => {
                this.getDataList();
                this.shDialog()
              }
            });
          } else {
            this.$message.error(data.msg);
          }
        });
      });
    },
    //批量审核窗口
    shBatchDialog(){
      this.sfshBatchVisiable = !this.sfshBatchVisiable;
      this.batchIds = this.dataListSelections.map(item => {
            return Number(item.id);
          });
    },
    //批量审核
    shBatchHandler(){
      this.$confirm(`确定一键审核操作?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$http({
          url: "kaoshibaoming/shBatch?sfsh="+this.shBatchForm.sfsh+"&shhf="+this.shBatchForm.shhf,
          method: "post",
          data: this.batchIds
        }).then(({ data }) => {
          if (data && data.code === 0) {
            this.$message({
              message: "操作成功",
              type: "success",
              duration: 1500,
              onClose: () => {
                this.getDataList();
                this.shBatchDialog()
              }
            });
          } else {
            this.$message.error(data.msg);
          }
        });
      });
    },
    // 下载
    download(file){
      window.open(`${file}`)
    },
    // 删除
    deleteHandler(id) {
      var ids = id
        ? [Number(id)]
        : this.dataListSelections.map(item => {
            return Number(item.id);
          });
      this.$confirm(`确定进行[${id ? "删除" : "批量删除"}]操作?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$http({
          url: "kaoshibaoming/delete",
          method: "post",
          data: ids
        }).then(({ data }) => {
          if (data && data.code === 0) {
            this.$message({
              message: "操作成功",
              type: "success",
              duration: 1500,
              onClose: () => {
                this.search();
              }
            });
          } else {
            this.$message.error(data.msg);
          }
        });
      });
    },


  }

};
</script>
<style lang="scss" scoped>
	
	.center-form-pv {
	  .el-date-editor.el-input {
	    width: auto;
	  }
	}
	
	.el-input {
	  width: auto;
	}
	
	// form
	.center-form-pv .el-input /deep/ .el-input__inner {
				border-radius: 0px;
				padding: 0 8px;
				box-shadow: 0 0 0px rgba(64, 158, 255, .5);
				outline: none;
				color: #666;
				background: #fff;
				width: auto;
				font-size: 14px;
				border-color: #b4e8d2;
				border-width: 4px;
				border-style: double;
				height: 40px;
			}
	
	.center-form-pv .el-select /deep/ .el-input__inner {
				border-radius: 0px;
				padding: 0 8px;
				box-shadow: 0 0 0px rgba(64, 158, 255, .5);
				outline: none;
				color: #666;
				background: #fff;
				width: auto;
				font-size: 14px;
				border-color: #b4e8d2;
				border-width: 4px;
				border-style: double;
				height: 40px;
			}
	
	.center-form-pv .el-date-editor /deep/ .el-input__inner {
				border-radius: 0px;
				padding: 0 4px 0 24px;
				box-shadow: 0 0 0px rgba(64, 158, 255, .5);
				outline: none;
				color: #666;
				background: #fff;
				width: auto;
				font-size: 14px;
				border-color: #b4e8d2;
				border-width: 4px;
				border-style: double;
				height: 40px;
			}
	
	// table
	.el-table /deep/ .el-table__header-wrapper thead {
				color: #812d58;
				font-weight: 500;
				width: 100%;
			}
	
	.el-table /deep/ .el-table__header-wrapper thead tr {
				background: #fff;
			}
	
	.el-table /deep/ .el-table__header-wrapper thead tr th {
				padding: 6px 10px;
				color: #333;
				background: none;
				border-color: #b4e8d2;
				border-width: 0px 1px 0px 0px;
				border-style: solid;
				text-align: left;
			}

	.el-table /deep/ .el-table__header-wrapper thead tr th .cell {
				padding: 0 10px;
				word-wrap: normal;
				word-break: break-all;
				white-space: normal;
				font-weight: bold;
				display: inline-block;
				vertical-align: middle;
				width: 100%;
				line-height: 24px;
				position: relative;
				text-overflow: ellipsis;
			}

	
	.el-table /deep/ .el-table__body-wrapper tbody {
				border: 0;
				width: 100%;
			}

	.el-table /deep/ .el-table__body-wrapper tbody tr {
				border: 0;
				background: none;
			}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td {
				padding: 4px 10px;
				color: #666;
				background: none;
				border-color: #b4e8d2;
				border-width: 0 1px 0px 0;
				border-style: solid;
				text-align: left;
			}
	
		.el-table /deep/ .el-table__body-wrapper tbody tr.el-table__row--striped td {
		background: rgba(180,232,210,.2);
	}
		
	.el-table /deep/ .el-table__body-wrapper tbody tr:hover td {
				padding: 4px 10px;
				color: #666;
				background: rgba(180,232,210,.3);
				border-color: #b4e8d2;
				border-width: 0 1px 0px 0;
				border-style: solid;
				text-align: left;
			}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td {
				padding: 4px 10px;
				color: #666;
				background: none;
				border-color: #b4e8d2;
				border-width: 0 1px 0px 0;
				border-style: solid;
				text-align: left;
			}

	.el-table /deep/ .el-table__body-wrapper tbody tr td .cell {
				padding: 0 10px;
				overflow: hidden;
				word-break: break-all;
				white-space: normal;
				line-height: 24px;
				text-overflow: ellipsis;
			}
	
	// pagination
	.main-content .el-pagination /deep/ .el-pagination__total {
				margin: 0 10px 0 0;
				color: #333;
				font-weight: 400;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				line-height: 28px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .btn-prev {
				border: 1px solid #efefef;
				border-radius: 2px;
				padding: 0;
				margin: 0 5px;
				color: #333;
				background: #fff;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				line-height: 28px;
				min-width: 35px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .btn-next {
				border: 1px solid #efefef;
				border-radius: 2px;
				padding: 0;
				margin: 0 5px;
				color: #333;
				background: #fff;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				line-height: 28px;
				min-width: 35px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .btn-prev:disabled {
				border: 1px solid #efefef;
				cursor: not-allowed;
				border-radius: 2px;
				padding: 0;
				margin: 0 5px;
				color: #333;
				background: #fff;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				line-height: 28px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .btn-next:disabled {
				border: 1px solid #efefef;
				cursor: not-allowed;
				border-radius: 2px;
				padding: 0;
				margin: 0 5px;
				color: #333;
				background: #fff;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				line-height: 28px;
				height: 28px;
			}

	.main-content .el-pagination /deep/ .el-pager {
				padding: 0;
				margin: 0;
				display: inline-block;
				vertical-align: top;
			}

	.main-content .el-pagination /deep/ .el-pager .number {
				cursor: pointer;
				border: 1px solid #efefef;
				padding: 0 4px;
				margin: 0 5px;
				color: #333;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				line-height: 28px;
				border-radius: 2px;
				background: #fff;
				text-align: center;
				min-width: 30px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .el-pager .number:hover {
				cursor: pointer;
				padding: 0 4px;
				margin: 0 5px;
				color: #fff;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				border-color: #5dae8b;
				line-height: 28px;
				border-radius: 2px;
				background: #5dae8b;
				text-align: center;
				min-width: 30px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .el-pager .number.active {
				cursor: default;
				padding: 0 4px;
				margin: 0 5px;
				color: #fff;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				border-color: #5dae8b;
				line-height: 28px;
				border-radius: 2px;
				background: #5dae8b;
				text-align: center;
				min-width: 30px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes {
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				line-height: 28px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes .el-input {
				margin: 0 5px;
				width: 100px;
				position: relative;
			}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes .el-input .el-input__inner {
				border: 1px solid #efefef;
				cursor: pointer;
				padding: 0 25px 0 8px;
				color: #333;
				display: inline-block;
				font-size: 13px;
				line-height: 28px;
				border-radius: 3px;
				outline: 0;
				background: #fff;
				width: 100%;
				text-align: center;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes .el-input span.el-input__suffix {
				top: 0;
				position: absolute;
				right: 0;
				height: 100%;
			}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes .el-input .el-input__suffix .el-select__caret {
				cursor: pointer;
				color: #333;
				width: 25px;
				font-size: 14px;
				line-height: 28px;
				text-align: center;
			}
	
	.main-content .el-pagination /deep/ .el-pagination__jump {
				margin: 0 0 0 24px;
				color: #333;
				display: inline-block;
				vertical-align: top;
				font-size: 13px;
				line-height: 28px;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .el-pagination__jump .el-input {
				border-radius: 3px;
				padding: 0 2px;
				margin: 0 2px;
				display: inline-block;
				width: 50px;
				font-size: 14px;
				line-height: 18px;
				position: relative;
				text-align: center;
				height: 28px;
			}
	
	.main-content .el-pagination /deep/ .el-pagination__jump .el-input .el-input__inner {
				border: 1px solid #efefef;
				cursor: pointer;
				padding: 0 3px;
				color: #333;
				display: inline-block;
				font-size: 14px;
				line-height: 28px;
				border-radius: 3px;
				outline: 0;
				background: #fff;
				width: 100%;
				text-align: center;
				height: 28px;
			}
</style>
